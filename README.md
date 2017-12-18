[![Packagist](https://img.shields.io/badge/language-python3.5-brightgreen.svg)]()  [![Packagist](https://img.shields.io/badge/platform-win--64%20%7C%20linux--64%20-lightgrey.svg)]()  [![DUB](https://img.shields.io/dub/l/vibe-d.svg)]()



# pastebin-API-python2-3
A Class for using the pastebin API in python 2 and 3.

# Usage

    > from paste_bin import PasteBinApi
    >
    > api        = PasteBinApi(dev_key="my secret api dev key")                       # Create api object
    > user_key   = api.user_key(username="my username", password="my password ")      # Create user key
    > link       = api.paste(user_key,                                               
                             title="my title",
                             raw_code="my code",
                             private=None,                                            # Returns link to new paste  
                             api_paste_format=None,
                             expire_date=None
                             ) 
    > trends        = api.trends()                                                    # Returns trending pastes 
    > pastes        = api.list_pastes(user_key=user_key, limit=50)                    # Returns n ammount of pastes from user
    > raw_paste     = api.get_raw(user_key=user_key, paste_key="Paste key")           # Returns raw paste data
    > user_info     = api.user_info(user_key=user_key)                                # Returns user info for user
    > delete_paste  = api.delete_paste(user_key=user_key, paste_key="Paste key")      # Deletes a paste

# Example
    
    ##########################################
    # Simple Pastebin Email:Password Scraper #
    ##########################################
    
    from paste_bin import PasteBinApi
    import argparse
    import re
    import requests

    api_dev_key = ""

    email_list = []

    api = PasteBinApi(api_dev_key)
    trends = api.trends()
    paste_key  = re.findall("\<paste\_key\>(.{1,50})\<", trends)

    print("[*] Scraping pastebin...\n")

    for key in paste_key:
         r = requests.get("https://pastebin.com/raw/{0}".format(key))
        data = r.text
        for line in data.split('\n'):
            if re.match("[\w.]+@[\w.]+:+", line):
                email_list.append(line)

    with open("email_dump.txt", "w")as f:
        for email in email_list:
            f.write(email+"\n")

    print("[*] Wrote {} Emails and Passwords to email_dump.txt".format(str(len(email_list))))
