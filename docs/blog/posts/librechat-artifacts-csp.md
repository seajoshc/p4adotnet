# LibreChat Artifacts Content Security Policy

I remote host [LibreChat](https://github.com/danny-avila/LibreChat) on a [Hetzner](https://www.hetzner.com/cloud/) VPS and I was trying to use the [Artifacts](https://www.librechat.ai/docs/user_guides/artifacts) feature, but I kept getting an [error](https://chatgpt.com/share/6725a275-0934-8006-aca3-b2d44cb23d34) when loading HTML5 apps in the iframe.

In the network tab on my browser's dev tools, I was seeing:

```
Refused to frame 'https://2-18-2-sandpack.codesandbox.io/' because it violates the following Content Security Policy directive: "default-src 'self'". Note that 'frame-src' was not explicitly set, so 'default-src' is used as a fallback.
```

The fix was adding a new content security policy to my [Caddyfile](https://caddyserver.com/) allowing the domain sandbox.io to load stuff in the iframe.

```
Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval'; style-src 'self' 'unsafe-inline'; frame-ancestors 'self' your.domain; frame-src 'self' https://*.codesandbox.io"
```

This appears to be a dependency of the [sandpack](https://sandpack.codesandbox.io/) library used by LibreChat to support Artifacts.

My Full Caddyfile config for LibreChat:

```
chat.your.domain {
    reverse_proxy localhost:3080
    header {
        Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval'; style-src 'self' 'unsafe-inline'; frame-ancestors 'self' your.domain; frame-src 'self' https://*.codesandbox.io"

        X-Frame-Options "SAMEORIGIN"
        X-Content-Type-Options "nosniff"
        Referrer-Policy "strict-origin-when-cross-origin"
        Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"
        Permissions-Policy "geolocation=(), midi=(), sync-xhr=(), microphone=(), camera=(), magnetometer=(), gyroscope=(), fullscreen=(self), payment=()"
    }
}
```
