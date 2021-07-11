# redian
#!/usr/bin/env python3
# -*- coding: utf-8-*-

import time
import hmac
import hashlib
import base64
import requests
import dingtalk

import json

import urllib
from urllib import parse


def redian():


    url='https://www.toutiao.com/api/pc/list/feed?channel_id=3189398996&min_behot_time=1625934341&refresh_count=4&category=pc_profile_channel&_signature=_02B4Z6wo02901uBOCHAAAIDA0Bg.5wW9Z1LgewzAANkHzKV128n4CSmvkjrIR515eDVyOWN8gbOYcFi31ZlPbUAssmnhzcXpkSdOmCEQADy83jiy6jWb-hDRpWzf-Og6SOa93xMYbaA7Be1Ub3'

    res=requests.get(url).text

    respond=json.loads(res)



    # 等有时间封装成一个for循环,先暂时这么处理

    title0=respond['data'][0]['title']

    display_url0=respond['data'][0]['display_url']

    try:

        image0=respond['data'][0]['large_image_list'][0]['url2']

    except BaseException:

        image0='https://p5.toutiaoimg.com/obj/eden-cn/uhbfnupkbps/video/earth_v6.jpeg'

    print(image0)

    title1=respond['data'][1]['title']

    display_url1=respond['data'][1]['display_url']

    try:

        image1=respond['data'][1]['middle_image']['url']

    except BaseException:

        image1='https://p5.toutiaoimg.com/obj/eden-cn/uhbfnupkbps/video/earth_v6.jpeg'



    title2=respond['data'][2]['title']

    display_url2=respond['data'][2]['display_url']

    try:

        image2=respond['data'][2]['middle_image']['url']

    except BaseException:

        image2='https://p5.toutiaoimg.com/obj/eden-cn/uhbfnupkbps/video/earth_v6.jpeg'



    title3=respond['data'][3]['title']

    display_url3=respond['data'][3]['display_url']

    try:

        image3=respond['data'][3]['middle_image']['url']

    except BaseException:

        image3='https://p5.toutiaoimg.com/obj/eden-cn/uhbfnupkbps/video/earth_v6.jpeg'


    title4=respond['data'][4]['title']

    display_url4=respond['data'][4]['display_url']

    try:

        image4=respond['data'][4]['middle_image']['url']

    except BaseException:

        image4='https://p5.toutiaoimg.com/obj/eden-cn/uhbfnupkbps/video/earth_v6.jpeg'
    
 # 时间戳

    timestamp = str (round (time.time () * 1000))
    # 钉钉加签
    secret = 'SECbb31344613f5730dad068081f967e65390c2368f230f5d4c357dd844ea50d193'
    secret_enc = secret.encode ('utf-8')
    string_to_sign = '{}\n{}'.format (timestamp, secret)
    string_to_sign_enc = string_to_sign.encode ('utf-8')
    hmac_code = hmac.new (secret_enc, string_to_sign_enc, digestmod=hashlib.sha256).digest ()
    sign = urllib.parse.quote_plus (base64.b64encode (hmac_code))
    print (timestamp)
    print (sign)
    # 钉钉机器人中的Webhook+sign+timestamp
    url = 'https://oapi.dingtalk.com/robot/send?access_token=849a0e39e5e07f7d176ebd7b27ca8a75ae7d550865ea5179b5be2fb57206f131&timestamp={0}&sign={1}'.format (timestamp, sign)
    
    
    headers={"Content-Type":"application/json;charset=utf-8"}
    

    data1={
        
    "msgtype":"feedCard",
        
    "feedCard": {
        
        "links": [
            
            {
                "title": title0, 
                "messageURL": display_url0, 
                "picURL": image0
            },
            {
                "title": title1, 
                "messageURL": display_url1, 
                "picURL": image1
            }
            ,
            
             {
                "title": title2, 
                "messageURL": display_url2, 
                "picURL": image2
            }
            
             ,
            
             {
                "title": title3, 
                "messageURL": display_url3, 
                "picURL": image3
            }

	   ,
            
             {
                "title": title4, 
                "messageURL": display_url4, 
                "picURL": image4
            }


            
        ]
    }
}

    data=json.dumps(data1)
    
#     print(type(data))

    response=requests.post(url,data=data,headers=headers)

    
    print(response.text)



if __name__ == '__main__':
    # 程序运行时调用方法
    redian()
