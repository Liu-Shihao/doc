

# 使用envoy filter重写请求头

要在 Istio 中将请求头 `x-citiportal-requestid` 的值覆盖到请求头 `x-request-id` 中，你可以使用 Envoy 的 Lua 过滤器来实现。以下是一个示例配置：

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: EnvoyFilter
metadata:
  name: request-header-override
spec:
  workloadSelector:
    labels:
      app: your-app
  configPatches:
    - applyTo: HTTP_FILTER
      match:
        context: SIDECAR_INBOUND
        listener:
          filterChain:
            filter:
              name: "envoy.http_connection_manager"
              subFilter:
                name: "envoy.router"
      patch:
        operation: INSERT_BEFORE
        value:
          name: envoy.lua
          typed_config:
            "@type": "type.googleapis.com/envoy.extensions.filters.http.lua.v3.Lua"
            inlineCode: |
              function envoy_on_request(request_handle)
                local x_citiportal_requestid = request_handle:headers():get("x-citiportal-requestid")
                if x_citiportal_requestid then
                  request_handle:headers():replace("x-request-id", x_citiportal_requestid)
                end
              end
```

在上面的配置中，我们使用了 Istio 的 EnvoyFilter 来修改请求头。这个配置会在请求进入 sidecar 代理时触发。

Lua 脚本中的 `envoy_on_request` 函数会获取请求头 `x-citiportal-requestid` 的值，如果存在的话，就用它来覆盖请求头 `x-request-id` 的值。

请注意，你需要将 `app: your-app` 替换为你的应用程序的标签选择器，以确保这个 EnvoyFilter 只会应用到你需要的服务。此外，确保已经启用了 EnvoyFilter 功能并且你的 Istio 版本支持 Lua 过滤器。