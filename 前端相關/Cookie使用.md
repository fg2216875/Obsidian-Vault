建立cookie
```C#
HttpCookie cookie = new HttpCookie(cookieName, value);
// 設定Cookie過期時間為隔日凌晨00:00
cookie.Expires = DateTime.UtcNow.Date.AddDays(1); 
//禁止Javascript存取這組Cookie
cookie.HttpOnly = true; 
Response.Cookies.Add(cookie);
```

前端讀取cookie值
```javascript
function getCookie(name) {
    const cookies = document.cookie.split(';');
    for (let i = 0; i < cookies.length; i++) {
        const cookie = cookies[i].trim();
        // 比對 cookie 名稱
        if (cookie.startsWith(name + '=')) {
            return cookie.substring((name + '=').length);
        }
    }
    return null; // 如果找不到，回傳 null
}
// 範例：讀取名為 "user" 的 cookie 
const userCookie = getCookie('user');
```

**1.無法直接讀取其他網域的 Cookie**
舉例來說，若用戶在 `example.com` 上設置了 cookie，該 cookie 只能在來自 `example.com` 的 HTTP 請求中自動附加。

2.
