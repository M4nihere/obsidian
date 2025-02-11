
---

##  Home Page API

### On Kill

```bash
106.219.68.27 - - [11/Feb/2025:08:36:00 +0000] "POST /hiferr/api/app-setting HTTP/1.1" 200 36004 "-" "okhttp/4.9.2"  
106.219.68.27 - - [11/Feb/2025:08:36:01 +0000] "POST /hiferr/api/get-post-list?page=1 HTTP/1.1" 200 97671 "-" "okhttp/4.9.2"  
106.219.68.27 - - [11/Feb/2025:08:36:02 +0000] "POST /hiferr/api/get-profile HTTP/1.1" 500 13950 "-" "okhttp/4.9.2"  
106.219.68.27 - - [11/Feb/2025:08:36:03 +0000] "POST /hiferr/api/post-view HTTP/1.1" 200 693 "-" "okhttp/4.9.2"  
106.219.68.27 - - [11/Feb/2025:08:36:03 +0000] "POST /hiferr/api/post-view HTTP/1.1" 200 1241 "-" "okhttp/4.9.2"  
106.219.68.27 - - [11/Feb/2025:08:36:04 +0000] "POST /hiferr/api/post-view HTTP/1.1" 200 7519 "-" "okhttp/4.9.2"  
106.219.68.27 - - [11/Feb/2025:08:36:03 +0000] "POST /hiferr/api/get-users-list-new?page=1 HTTP/1.1" 200 197165 "-" "okhttp/4.9.2"  
```

---

## Discover Page

### Questions and Observations

1. **On Kill**: The app calls the `api/get-users-list-new`, but when clicking on Discover, it calls `api/get-users-list-new` again. Why?
    
2. **Duplicate Calls**: Why is `/hiferr/api/post-view` being called more than once?
    
3. **Order of API Calls**: Can't we call `/hiferr/api/get-post-list` before any other API?
    
4. **Search Behavior**: On the Discover Page:
    
    - When searching for text, the app calls `api/get-users-list-new` for every character typed in rapid succession.
    - It should wait at least 1 second before making the API call.
    - If results are already available, the app should not call `api/get-users-list-new` again upon clicking the search button.
5. **Enable/Disable Location**: When toggling "Discover by location" on the Discover Page, the following APIs are called:
    
    ```bash
    106.219.68.27 - - [11/Feb/2025:09:08:07 +0000] "POST /hiferr/api/user-distance-value HTTP/1.1" 200 3356 "-" "okhttp/4.9.2"  
    106.219.68.27 - - [11/Feb/2025:09:08:08 +0000] "POST /hiferr/api/get-profile HTTP/1.1" 500 13950 "-" "okhttp/4.9.2"  
    106.219.68.27 - - [11/Feb/2025:09:08:09 +0000] "POST /hiferr/api/get-users-list-new?page=1 HTTP/1.1" 200 29532 "-" "okhttp/4.9.2"  
    ```
    
    **Question**: Why does it call `/hiferr/api/get-profile`?
    
6. **Back Navigation**:
    
    - When on the Discover Page, clicking on a profile and then returning via the back button triggers the following API call:
        
        ```bash
        106.219.68.27 - - [11/Feb/2025:09:43:17 +0000] "POST /hiferr/api/get-post-list?page=1 HTTP/1.1" 200 97701 "-" "okhttp/4.9.2"  
        ```
        
        Is this necessary?
7. **Profile Click**:
    
    - Clicking on a profile from the Discover Page triggers these API calls:
        
        ```bash
        106.219.68.27 - - [11/Feb/2025:09:12:12 +0000] "POST /hiferr/api/get-profile-new HTTP/1.1" 200 27913 "-" "okhttp/4.9.2"  
        106.219.68.27 - - [11/Feb/2025:09:12:13 +0000] "POST /hiferr/api/my-posts-new?page=1 HTTP/1.1" 200 20844 "-" "okhttp/4.9.2"  
        106.219.68.27 - - [11/Feb/2025:09:12:16 +0000] "POST /hiferr/api/post-view HTTP/1.1" 200 693 "-" "okhttp/4.9.2"  
        ```
        
        **Note**: Why does it call `/hiferr/api/post-view`?

---

## Inbox Page
### On App Open (Kill State)

```bash
4|hiferr  | Event received: get_user_detail [  
4|hiferr  | Event received: get-conversations [  
4|hiferr  | Event received: get_user_detail [  
```

### Clicking on Inbox

```bash
4|hiferr  | Event received: get-conversations [  
4|hiferr  | Event received: get_recent_users [ '', [Function (anonymous)] ]  
4|hiferr  | Event received: get-request-conversations [  
```

### Chat Messages

- When there are messages for a user in the chat and the chat is opened, the following API is called three times:
```bash
4|hiferr  | Event received: get-messages [  
4|hiferr  | Event received: get-messages [  
4|hiferr  | Event received: get-messages [  
```
    

---

# Elastic Search

- Elastic Search can be implemented on the backend to optimize API performance and response times.

---
