If you didn't do the **1st Part**, go [back](/courseFiles/Section_06-webSecurity/webLabPart1.md)


### We have **4 routes**:
- ``/`` - Vulnerable login
- ``/secure`` - Secure login
- ``/logs`` - View the lab's access logs
- ``/init`` - Reset DB

### Going into Microsoft Edge you can navigate to to http://10.10.119.212:8000/init
- You will get some credentials like these that will be in the DB
<img width="479" height="80" alt="image" src="https://github.com/user-attachments/assets/de26d4a4-49ee-42e8-a2b8-be5f9645c2a0" />

- Now go over to ``http://10.10.119.212:8000`` ( the vulnerable login )

- You can enter whatever username and password you want and it will fail since there is no instance of that in the DB
<img width="444" height="445" alt="image" src="https://github.com/user-attachments/assets/94f196fc-54e3-4815-98c4-5b4e7d21b6bf" />

- However... If we use ``' OR '1'='1' --`` as our username and whatever password it will bypass any other checks!
<img width="417" height="462" alt="image" src="https://github.com/user-attachments/assets/a5042ca5-e8b3-46b3-a6d9-655df8660c98" />

- We can now see the logs at ``http://10.10.119.212:8000/logs``
<img width="1465" height="94" alt="image" src="https://github.com/user-attachments/assets/2703d3b0-704e-4671-b630-9c95b8e3b547" />

- A SOC analyst seeing this will instantly think of SQLi, let's go review the source code that I have you in the [Part 1](/courseFiles/Section_06-webSecurity/webLabPart1.md)

- In the vulnerable code, this query is sent:

<pre>SELECT * FROM users
WHERE username = '<USER_INPUT>' AND password = '<USER_INPUT>';</pre>

**Why is this bad?**

Because the database doesn’t know where “data” ends and “SQL syntax” begins

If the input contains quotes, operators, or comments, it can change the meaning of the query

A **before** and **after** would look like this for the query

``SELECT * FROM users WHERE username='admin' AND password='secret';`` - **Intended**

``SELECT * FROM users WHERE username='' OR '1'='1' -- ' AND password='';`` - **With Payload**

**Result:** ``WHERE`` clause becomes always true -> first row returned -> login succeeds

<br><br>

- Why the secure code works?
1. We prepare the SQL with placeholders (?, :u, :p)
2. We **bind** the user inputs as **data**, not as SQL
3. The database **parses** the query structure first, then **injects the values** safely

**Bindings:**
<pre>:u = "admin' OR '1'='1' --"
:p = "anything"</pre>

Because they’re bound as values, the quotes and comment markers are treated as text, not syntax

The WHERE clause remains exactly what we wrote: ``username = <value> AND password = <value>``

- Now go over to ``/secure`` to see that it doesn't work anymore

<img width="417" height="399" alt="image" src="https://github.com/user-attachments/assets/e4e7f1e3-d371-4cb0-95b1-45abcb179862" />

- Let's see the logs again at ``/logs``

<img width="754" height="22" alt="image" src="https://github.com/user-attachments/assets/7f1485d7-7899-4715-8a48-6e54e1251ed8" />

## TL;DR

### Why /vuln is vulnerable
- User input directly concatenated into SQL
- Database can’t tell syntax from data
- Payload rewrites the query -> bypasses auth

### Prepared statements separate SQL code from user data
- Prepared statements separate SQL code from user data
- Input is always treated as text, never as instructions
- Query structure stays intact no matter the payload

### SOC Detection Tips
- Watch for ``OR 1=1``, ``--``, ``UNION SELECT``, ``sleep()`` in logs
- Repeated failed logins followed by success with odd input
- SQLi tools (sqlmap) leave signatures in User-Agent and timing

Also a real world example is [Equifax 2017](https://en.wikipedia.org/wiki/2017_Equifax_data_breach) where there was a Data Breach of 147M Records because of SQLi

---
[Back to the section](/courseFiles/Section_06-webSecurity/webSecurity.md)


