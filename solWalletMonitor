import requests
import json
import smtplib, ssl
from bs4 import BeautifulSoup
import os
#you can change to whatever smtp server you want
smtp_server = "smtp.gmail.com"
headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.77 Safari/537.36"}
#Fill in your Wallet Address
walletAddress = ""

#Fill in the gmail account you are sending from
gmailAccount = ""
#Your gmail account password   
gmailLogin = ""
#Email Address to send to,  I send to my version phone email adress which then sends a text
sendEmailAccount = ""

r = requests.get("http://api.solscan.io/account/tokens?address="+walletAddress+"&price=1", headers=headers)
rc = r.status_code

soup = BeautifulSoup(r.content, 'html.parser')
print('*******************\n')
data = json.loads(soup.text)

tokenCount = int(0)

for val in data['data']:
    print("Token Amount:" + val['tokenAmount']['amount'] + " Token Account:" + val['tokenAccount'] + "\n")

    if val['tokenAmount']['amount'] != "0":
        tokenCount = int(tokenCount) + int(val['tokenAmount']['amount'])

x = [0,0]
r = requests.get("http://api.solscan.io/account?address="+walletAddress+"&price=1", headers=headers)

rc = r.status_code
soup = BeautifulSoup(r.content, 'html.parser')
data = json.loads(soup.text)
walletBalance = 0

if len(data['data']) > 1:
    walletBalance = data['data']['lamports']
print("Balance: ", walletBalance)

file_exists = os.path.exists(walletAddress + '.txt')
fileData = ""
if file_exists:
    with open(walletAddress + '.txt', "r+") as myfile:
        fileData = myfile.read()
        myfile.seek(0)
        myfile.close()

x = fileData.split(",")

print("len = " + str(len(x)))
if len(x) == 1:
    x = [0,0]

print("-" + str(x[1]).strip() + "-" + str(tokenCount).strip())
printWallet = walletAddress[-4] + walletAddress[-3] + walletAddress[-2] + walletAddress[-1]
if len(fileData) > 0:
    if str(x[0]).strip() != str(walletBalance).strip() or str(x[1]).strip() != str(tokenCount).strip():
            server = smtplib.SMTP(smtp_server, 587)
            server.starttls()
            server.login(gmailAccount, gmailLogin)
            server.sendmail(fromEmailAccount, sendEmailAccount, "Wallet Address: " + printWallet + "\n Sol Balance:  " + str(walletBalance) + " Sol Token Count:    " + str(tokenCount) + "\n Prev Balance:" +str(x[0]) + " Prev Token Count: " + str(x[1]))
            print("BALANCE CHANGED\n Sol Wallet Balance:" + str(walletBalance) + " Sol Wallet Token Count:" + str(tokenCount) + "\n\n")

dataLine =  str(walletBalance) + ", " + str(tokenCount)

with open(walletAddress + '.txt', "w+") as myfile:
    myfile.write(dataLine)
    

