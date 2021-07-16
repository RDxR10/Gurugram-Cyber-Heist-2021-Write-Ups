* As usual, there is a login page: https://chall2.hackingbrawl.com/newlogin.php (This challenge was the next in the sequel to the basic sqli one)

* At first few SQL injection methods were attempted, but these went in vain. Upon using sqlmap, it was discovered that it was vulnerable `time-based-blind` injection. So manual attempts were done, the site seemed to slow down a bit when the payload was injected. 
* The `data` value required for sqlmap was found upon doing a POST request.
* the lower levels did not work, so the level was increased and tested.
* `hack_chall2` was the database, `TAB` was the table, with `PASSWORD` and `USERNAME` as field(s).
* The flag was in one of the rows of the table. `CTF{INJECTKAFLAG}`
