---
layout: post
title:  "OWASP Top 10: Injection"
---

The Open Web Application Security Project (OWASP) constantly maintains a list of concerns regarding web application security.

The OWASP Top 10 is focuses on the most critical and commonly occurring risks in web application security. The latest update in this project is the *OWASP Top 10 - 2017 report*. This series will focus on introducing readers to the concepts of these web application threats, as well as talk about countermeasures and prevention methodology.

<img src="/assets/Injection/1.jpg" alt="OWASP Injection" width="500"/> 
*Credits: **Linux Security Blog** (https://linuxsecurityblog.com/2017/11/23/owasp-a1-injection-cause-and-prevention/)*

**Fundamentals of Injection**

*Attack Vectors*

An injection attack occurs in web applications where untrusted data is passed through an interpreter without adequate code sanitisation. Most likely this is where the application's input expects one form of data, but the input is not secured from alternatives, leading to an attack being able to inject malicious code.

Thus, any source of data can be affected by injection attacks. Some examples include:
 - Environment variables
 - Internal Services and External Web Services (such as SQL Injection etc.)

*Security Weakness*

Injection-related vulnerabilities are very prevalent in modern web applications primarily because of the continued usage and lack of santisation of legacy code, as well as the minimal security focus in application programming. Injection flaws can easily be detected by most vulnerability scanners, as well as through manual code examination.

*Impact*

The impact possibilities that can stream from injection can be severe. Some injections can result in takeover of certain servers, which may then lead to the data breach, disclosure of confidential or private information and denial of access. This may greatly affect the confidence and integrity of businesses if information is leaked.

**Types of Injection**

More detailed explanations of the types of injections will be discussed in future posts. The most common type of injections include:

*Query Injection*

 - **SQL Query Injection**: The attack which a user can inject parts or all of an SQL query, and the data will be processed and executed by the web application.
 - **LDAP Injection**: The modification of LDAP statements in a similar fashion to SQL Injection. This may result in the modification of content outside the scope of LDAP.
 - **XPath Injection**: The attack technique used to exploit XML Path Language queries that are normally used to navigate XML documents.

 *Scripting Language Injection*

 - **Null Byte Injection**: The actual implementation of higher scripting languages is handled by lower languages. A flaw in the handling will allow for attack vectors, such as placing the null byte '\0' to terminate execution of code.

 *Operating System Injection*

 - **Command Injection**: A web interface can communicate to a web server and execute operating system commands, such as a bash command. Improper santisation will allow direct access to shell commands.

 **Prevention Methods**

 1. **Input validation**: whitelist input validation to only provide the user with the characters that are necessary for the task, no more, no less.

 2. **Escape user data**: escape special characters for the interpreter where necesary.

 3. **Use secure APIs**: so that there is no need for interpretation, however in some circumstances these can also contain injection attack vectors.