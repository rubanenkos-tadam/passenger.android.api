HMAC Authorization
===

All request, wich required authorization must have **Headers**

Header | Description
------ | --------
Date | Current datetime in RFC-1123 format
Authentication | hmac {identity}:{nonce}:{digest}

* {identity} - mobile app **id**
* {nonce} - numeric value, must be unique for each request
* {digest} - message signature, base64encode(hmac("sha256", {secret}, "{method}{path}{date}{nonce}"))
  * {secret} – base64decode("{key}") (**key** - received by registration).
  * {method} – request method (ex. GET).
  * {path} – request path (ex. /api/client/mobile/1.0/history).
  * {date} – same as **Date** header.
  * {nonce} – see above.
  
Example
===
**Request data**
* identity: 45000331513082
* secret/key: e0wkIP7JjlgJh7+HrLrQ8Lkgpx5Cp//A0n3PAVU1JWo=
* method: GET
* path: /api/client/mobile/3.0/estimate
* Date: Mon, 16 Mar 2020 13:55:54 +0600
* Authentication: hmac 45000331513082:979098767:i4qNxHnmD8QzxN4azqKZ/8BLY+MJl/Dx37E2+6ubijE=

---

### Java 1.8


```java
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.nio.charset.StandardCharsets;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;
import java.time.OffsetDateTime;
import java.util.Base64;
import java.util.HashMap;
import java.util.Map;

import static java.time.format.DateTimeFormatter.RFC_1123_DATE_TIME;

public class NaiveHmacSigner {

    public static byte[] hmac(String algorithm, byte[] secret, String data)
        throws NoSuchAlgorithmException, InvalidKeyException {

        final Mac mac = Mac.getInstance(algorithm);
        final SecretKeySpec spec = new SecretKeySpec(secret, algorithm);
        mac.init(spec);
        return mac.doFinal(data.getBytes(StandardCharsets.UTF_8));
    }


    private final String identity;
    private final byte[] secret;

    public NaiveHmacSigner(long id, String key) {
        this.identity = String.valueOf(id);
        this.secret = Base64.getDecoder().decode(key);
    }

    public Map<String, String> newSignature(String method, String path)
        throws InvalidKeyException, NoSuchAlgorithmException {

        final String date = OffsetDateTime.now().format(RFC_1123_DATE_TIME);
        final String nonce = String.valueOf(System.currentTimeMillis());

        final String data = method + path + date + nonce;

        final String digest = Base64.getEncoder().encodeToString(
            hmac("HmacSHA256", this.secret, data)
        );

        final Map<String, String> result = new HashMap<>();
        result.put("Date", date);
        result.put("Authentication", String.format("hmac %s:%s:%s", this.identity, nonce, digest));
        return result;
    }


    public static void main(String[] args) throws NoSuchAlgorithmException, InvalidKeyException {

        NaiveHmacSigner signer = new NaiveHmacSigner(12312313, "beKXDqRvkrbz+aQpEgn41SSh+9qtLAsb0r2cbcQ24cM=");
        Map<String, String> signature = signer.newSignature("GET", "/api/client/mobile/1.0/history");

        System.out.println(signature);
    }
}
```
