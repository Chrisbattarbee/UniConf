# UniConf
Uniconf is an open source configuration language for CDN management across multiple CDN providers.
The idea is that you can define your CDN configuration once in UniConf then have CDN specific configurations generated from it.
This enables multi-cdn deployments to be managed from a single source of truth.

## UniEdge

UniConf is the configuration language for UniEdge, a multi-cdn edge platform that allows you to deploy your applications across multiple CDN providers.

## Documentation
The language is designed to be simple and is documented in the [UniConf Language Specification](./docs/language.md).

### Example

```yaml
# Preference: "RequireEnabled" | "PreferEnabled" | "PreferDisabled" | "RequireDisabled"

# UniEdge.io
Backend:
  OriginService: 
    Host: "multi-cdn-website.vercel.app"
    Protocol: "https"
    Port: 443
  CustomDomains:
    - "uniedge.io"

ClientRequest:
  ForceHttpsRedirect:
    Preference: "RequireEnabled"
    Value: true
  Compression:
    Preference: "PreferEnabled"
    Value: "AnyCompression" 

Caching:
  CachePolicies:
    NonDefault:
      Preference: "RequireEnabled"
      Policies:
        API:
          SuccessfulResponseCaching:
            Preference: "RequireDisabled"
          ErrorResponseCaching:
            Preference: "RequireDisabled"
          Match:
            PathPattern: "/api/*"
            Method: "GET"
            QueryParameters:
              - "page"
              - "limit"
              - "sort"
            Headers:
              - "Authorization"
          CustomCacheKey:
            Preference: "RequireDisabled"
    Default:
      SuccessfulResponseCaching:
        Preference: "RequireEnabled"
        DefaultTTL:
          Preference: "PreferEnabled"
          Value: 3600
      ErrorResponseCaching:
        Preference: "RequireDisabled"
      CustomCacheKey:
        Preference: "RequireEnabled"
        RequestHeaders:
          - "User-Agent"
          - "Accept-Language"
        RequestQueryParameters:
          - "utm_source"
          - "utm_medium"
          - "utm_campaign"
        RequestCookies:
          - "session_id"
```