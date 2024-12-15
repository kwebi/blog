# 超能犇信号断网自动切换脚本


## 逆向超能犇的登录api

首先可以从网页端逆向js，不难推出其使用hex_hmac_md5算法对登录用户名和密码进行了加密。并抓包其登录api和切换网络优先级的api。

- 运行逻辑如下：不断用ping命令监测是否断网，间隔几秒监测一次。
- 如果失败，判断是否在8-24点之间，如果是（则切换为仅sa，再测一次，如果断网切换4g）
- 如果不在8-24点，则切换为4g。
- 后续再监测一次，如果还是断网，则切换回自动，暂停10分钟

我的脚本跑在n1盒子上，和cpe用网线连接，用青龙面板定时运行。一般不会出现登录失败的情况。如果切换网络失败，进行重新登录又失败，则会暂停1小时。

需安装python，并手动执行: `pip install requests`

代码中的 `username` 改为自己的用户名, `password` 改为自己的密码, `host` 改为自己的路由器ip, 详细见代码注释

```python
import requests
import json
import datetime
import time
import os
import hmac
import hashlib
# 需手动执行: pip install requests
#           pip install pyexecjs


#双引号内改为自己的用户名
username = "admin"
#双引号内改为自己的密码
password = "admin"
#双引号内改为自己的路由器ip
host = "192.168.88.1"


headers = {
    "Accept": "application/json, text/javascript, */*",
    "Accept-Encoding": "gzip, deflate",
    "Accept-Language": "zh-CN, zh",
    "Cache-Control": "no-cache",
    "Connection": "keep-alive",
    "Content-Type": "application/json",
    "Host": "192.168.88.1",
    "Origin": "http:// 192.168.88.1",
    "Pragma": "no-cache",
    "Referer": "http://192.168.88.1/html/settings.html",
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0Win64x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.6261.95 Safari/537.36",
}

s = requests.Session()

s.headers.update(headers)

key: str = "0123456789"


# 测试网络连接
url = "http://www.baidu.com"
url2 = "qq.com"


def hex_hmac_md5(key, msg):
    """
    计算使用MD5散列算法的HMAC，并返回十六进制字符串形式的结果。
    :param key: HMAC密钥
    :param msg: 消息
    :return: 十六进制HMAC值
    """
    hmac_digest = hmac.new(key.encode(
        'utf-8'), msg.encode('utf-8'), hashlib.md5)
    return hmac_digest.hexdigest()


username = hex_hmac_md5(key, username)
password = hex_hmac_md5(key, password)

def login(username: str, password: str) -> bool:
    data = {'username': username, 'password': password}

    response = s.post(url=f"http://{host}/goform/login", headers=headers,
                      data=json.dumps(data))
    print(response.text)
    try:
        ret = json.loads(response.text)
        if(ret['retcode'] != 0):
            return False
    except Exception as e:
        return False
    return True


def mnet_set_prefer_net_mode(type: int):

    if type == 0:
        data = r'{"mnet_acqorder":"auto","mnet_band":"auto","mnet_scan_mode":"auto"}'
    elif type == 1:
        data = r'{"mnet_acqorder":"sa","mnet_band":"auto","mnet_scan_mode":"auto"}'
    elif type == 2:
        data = r'{"mnet_acqorder":"4g only","mnet_band":"auto","mnet_scan_mode":"auto"}'
    response = s.post(url=f"http://{host}/action/mnet_set_prefer_net_mode",
                      headers=headers, data=data)
    print(response.text)
    try:
        ret = json.loads(response.text)
        if(ret['retcode'] != 0):
            return False
    except Exception as e:
        #失败重新登录
        is_login_sucess = login(username, password)
        if(is_login_sucess):
            print("登录成功")
            response = s.post(url=f"http://{host}/action/mnet_set_prefer_net_mode",
                              headers=headers, data=data)
            print(response.text)
        else:
            print("登录失败")
            time.sleep(3600)
    


def test_network(url=url, timeout=1):
    try:
        response = requests.get(url, timeout=timeout)
        if response.status_code == 200:
            return True
        else:
            return False
    except requests.exceptions.RequestException:
        return False



def is_connected_to_internet(host=url2):
    """
    检查设备是否连接到互联网。
    使用谷歌的DNS服务器进行ping测试。
    """
    if(os.name == "nt"):
        response = os.system(f"ping  {host}")
    else:
        response = os.system(f"ping -c 1  {host}")
    if response == 0:
        return True
    else:
        return False


def test()->bool:
    
    start_run_time = time.time()
    end_run_time = time.time()+3585
    
    is_login_sucess = login(username, password)
    if(is_login_sucess):
        print("登录成功")
    else:
        print("登录失败")

    exec_cnt: int = 0  # 测试次数
    flag: bool = False  # 是否白天失败切换4g
    while True:
        #运行超过1小时，结束
        if(time.time()>end_run_time):
            break
        # 范围时间
        start_time = datetime.datetime.strptime(
            str(datetime.datetime.now().date()) + '8:00', '%Y-%m-%d%H:%M')
        end_time = datetime.datetime.strptime(
            str(datetime.datetime.now().date()) + '23:59', '%Y-%m-%d%H:%M')
        # 当前时间
        now_time = datetime.datetime.now()
        # 如果断网
        if not is_connected_to_internet():
            print("网络异常")
            # 判断当前时间是否在范围时间内，切换仅5g
            if start_time < now_time < end_time:
                print("当前时间在范围时间内, 切换仅5g")
                mnet_set_prefer_net_mode(1)
                time.sleep(5)
                # 网络测试失败,切换仅4g
                if is_connected_to_internet() == False:
                    print("#网络测试失败,切换仅4g")
                    mnet_set_prefer_net_mode(2)
                    flag = True
                    exec_cnt = 0
                    time.sleep(5)
                if not is_connected_to_internet() and flag:
                    print("4g网络异常, 恢复自动, 休息10分钟")
                    mnet_set_prefer_net_mode(0)
                    time.sleep(600)
            else:
                #不在时间范围内, 直接切换4g
                print("不在时间范围内, 直接切换4g")
                mnet_set_prefer_net_mode(2)
                time.sleep(5)
                flag = True

            if is_connected_to_internet() == False:
                print("网络还是异常, 恢复自动, 休息10分钟")
                mnet_set_prefer_net_mode(0)
                time.sleep(600)
        else:
            print("网络正常")

        # 暂停
        time.sleep(8)
        exec_cnt = exec_cnt+1
        if exec_cnt > 200:
            print("循环200次, 已经运行{}秒, 恢复自动, 休息1分钟".format(
                time.time()-start_run_time))
            mnet_set_prefer_net_mode(0)
            time.sleep(60)
            


if __name__ == '__main__':
    test()


```
