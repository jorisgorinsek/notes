Brucon talk on API to API hacking:[](https://www.youtube.com/watch?v=Ddl4j3WL4UA)

[05 - BruCON 0x10 - Bypassing firewalls with API-to-API Hacking - Johan Caluwé](https://www.youtube.com/watch?v=Ddl4j3WL4UA)

### WAF bypasses

- not triggering the WAF in single calls to API → example valid calls but omitting some of the payload (removing the key instead of changing it). Crazy but it worked.
    
- No payload = no triggering of the WAF
    
- Trust relation between APIs behind the WAF → they assume that request passing the WAF are safe
    
- Transformations
    
    - Every hop in the chain, a transformation can happen (JSON parsing, encoding, decoding …)
        
    - e.g. JSON parser will transform UNICODE to ASCII (for latin characters) because that is the more compatible way to represent these chars
        
- Using transformation chaining to bypass WAF
    
    - double url encoding (WAFs stop this) → 2 HTTP stacks that both do a single pass of HTTP decoding
        
    - WAFs can detect this because this stays within the same encoding type (e.g. HTTP encoding). But they cannot anticipate future Content-types
        
    - → unicode representation of the % sign (\u0025) followed by the HTTP encoding of / “2F”
        
    - smuggling of API requests
        
    - stripping appended hard-coded patch by including a $ or ? in the request
        

### API-to-API hacking

- APIs use other APIs too
    
- target is one of the secondary APIs
    
- one object (e.g. JSON) can contain data that is fetched from multiple secondary APIs
    
- parameter values of request are forwarded to secondary APIs
    
- The first API does not always validate the parameter. HINTS are in multiple status codes (e.g. 200 from 1st API, another status code in the body).
    
- SSRF example → make the second API do something[](https://youtu.be/Ddl4j3WL4UA?t=847)
    

- [05 - BruCON 0x10 - Bypassing firewalls with API-to-API Hacking - Johan Caluwé](https://youtu.be/Ddl4j3WL4UA?t=847)
    

### Increasing the attack surface

- 1st API and 2nd API can use different technologies with different vulnerabilities
    
- The communication between API 1 and 2 is an attack surface on itself!
    
- For us, the 1st API is a server, for the 2nd API, it is a client!