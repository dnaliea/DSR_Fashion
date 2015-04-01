'''Use the instagram API to pull pictures from instagram (306x306)'''

from urllib2 import urlopen
from urllib2 import HTTPError
import urllib
from json import load 
from pprint import pprint
import re
import requests
from PIL import Image
from StringIO import StringIO
import time
import matplotlib
import numpy as np
import matplotlib.pyplot as plt

#%matplotlib inline


#my API Credentials
ACCESS_token = "419637133.56ab991.85ad59ca6b664fd28c7c5a0690510596"
user_id = "273346042" #my user id
#https://api.instagram.com/v1/users/{user-id}/media/recent/?access_token=ACCESS-TOKEN


#########FIND USR IDS#####################

def findFollowerIDs(user_id, access_token):
#get user id of interesting instagram users #(followed by me)

#assemble url
    url = 'https://api.instagram.com/v1/users/' 
    user_id = user_id
    url2 = '/follows?access_token='
    #min_timestamp =
    #max_timestamp = 
    access_token = ACCESS_token
    url = url + user_id + url2 + access_token
    print url
    
   # url="https://api.instagram.com/v1/users/273346042/follows?access_token=419637133.56ab991.85ad59ca6b664fd28c7c5a0690510596"
    response = urlopen(url)
    json_obj = load(response)
    id_list = []

    length = len(json_obj["data"])

    for i in range(0,length):
        user_id = json_obj["data"][i]["id"] # id
        user = json_obj["data"][i]["username"] # username
        id_list.append(user_id) # username   
        
    return id_list
####################################################

####################GET RECENT PICS FROM ONE USER##############################
def _getAllData_test(id):
    '''Gets Data from ONE user. id= numeric instagram ID, pagination works '''
    
    #assemble initial URL for HTTPS-API REQUEST
    url = 'https://api.instagram.com/v1/users/' 
    xuser_id = str(id)
        #print xuser_id
    url2 = '/media/recent/?access_token='
    access_token = ACCESS_token
    #ncount = ncount
    #min_timestamp = 1.August
    #max_timestamp = 1. Oktober
    url = url + xuser_id + url2 + access_token
    #print url
    
    #initialise lists
    _user = []
    _usr_id =[]
    _picUrl = []
    _hashtags = []
    _timestamp = []
    #_caption = []
    _likecount = []
    _pic_id = []
    
    #deal with HTTP 400 errors if User is private
    try:
        global raw
        raw = urlopen(url)
        #print xuser_id+":"+" success"
        #return response      
    except HTTPError, error: 
        print xuser_id+" is private, no images loaded"
        return None  # return nothing
    
    else: 
    #retrieve JSON object containing image URLs and, 
    #possibly, the next page URL    
        response = urlopen(url)
        json_obj = load(response)
        data = json_obj["data"]
        
        for i in range(1,len(data)): 
            _user.append(data[i]["user"]["username"]) #username,
            _usr_id.append(data[i]["user"]["id"]) #user-id
            _picUrl.append(data[i]["images"]["low_resolution"]["url"]) #lowres-url
            #_hashtags.extend(data[i]["data"][i]["tags"]) #hashtags
            _timestamp.append(data[i]["created_time"]) #hashtags
            #_timestamp[i]= datetime.datetime.fromtimestamp(float(_timestamp[i]))
            #_caption.extend(data[i]["caption"]["text"])
            _likecount.append(data[i]["likes"]["count"])
            _pic_id.append(data[i]["images"]["low_resolution"]["url"].rsplit('/',1)[1] )        
            #print len(_pic_id)
            
        
        counter = 1
        #print counter
        while url !={} and counter < 5:
            try: next_url = json_obj["pagination"]["next_url"]
            except KeyError, error:
                counter = 6
            else:
                #print next_url
                url = next_url
                response = urlopen(url)
                json_obj = load(response)
                data = json_obj["data"]
                    
                for i in range(1,len(data)): 
                    _user.append(data[i]["user"]["username"]) #username,
                    _usr_id.append(data[i]["user"]["id"]) #user-id
                    _picUrl.append(data[i]["images"]["low_resolution"]["url"]) #lowres-url
                    #_hashtags.extend(data[i]["data"][i]["tags"]) #hashtags
                    _timestamp.append(data[i]["created_time"]) #hashtags
                    #_timestamp[i]= datetime.datetime.fromtimestamp(float(_timestamp[i]))
                    #_caption.extend(data[i]["caption"]["text"])
                    _likecount.append(data[i]["likes"]["count"])
                    _pic_id.append(data[i]["images"]["low_resolution"]["url"].rsplit('/',1)[1] )
                    #print len(_pic_id)
                    
                counter +=1
            #print counter 
                
    NumPics = len(_picUrl)
    #print NumPics
    usrname = data[i]["user"]["username"]
    min_time = min(_timestamp)
    min_time = datetime.date.fromtimestamp(float(min_time))
    max_time = max(_timestamp)
    max_time = datetime.date.fromtimestamp(float(max_time))
        
    print "data for " + str(NumPics) + " pictures from user " +usrname+ " received - "  +   str(min_time)+ " to " + str(max_time)
  
    return {'user': _user, 
            'usr_id': _usr_id,
            'timestamp': _timestamp
            ,'likecount': _likecount
            ,'hashtags': _hashtags
            ,'url': _picUrl
            ,'pic_id': _pic_id } 
            
############################################################################

####### GET ALL DATA FROM A LIST OF USERS (IDS) ''''''''''''''

def getAllDataFromIDList(ids):
#Getting data from all users in "Ids".

print 'Getting data from '+ str(len(ids))+ ' users'

for i in range(0,len(ids)):   
    id = ids[i]
    user = []
    
    #print id
    info = _getRecentPics(id)
    
    if info == None:
        user.extend([])
    else :   
        user.extend(info['user'])
    
print "All done!"

###############################################
        
    
