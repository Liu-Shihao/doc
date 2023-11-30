如果在过滤器中读取了 `HttpServletRequest` 的输入流，并且在打印了 URL 和 Request Body 之后仍然遇到 I/O 错误，你需要确保在传递给下一个过滤器之前，将输入流内容重新设置为原始状态，以便后续的过滤器能够读取它。

下面是一个可能的解决方法：

```java
import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;

public class YourFilter implements Filter {

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        // 读取输入流内容
        String requestBody = readRequestBody(request);

        // 打印 URL 和 Request Body
        System.out.println("URL: " + ((HttpServletRequest) request).getRequestURL());
        System.out.println("Request Body: " + requestBody);

        // 将输入流重新设置为原始状态
        request = resetInputStream(request, requestBody);

        // 通过过滤器链传递请求
        chain.doFilter(request, response);

        // 继续对 response 进行操作
    }

    private String readRequestBody(ServletRequest request) throws IOException {
        StringBuilder requestBody = new StringBuilder();
        try (BufferedReader reader = request.getReader()) {
            char[] charBuffer = new char[1024];
            int bytesRead;
            while ((bytesRead = reader.read(charBuffer)) != -1) {
                requestBody.append(charBuffer, 0, bytesRead);
            }
        }
        return requestBody.toString();
    }

    private ServletRequest resetInputStream(ServletRequest request, String requestBody) {
        // 创建新的包含请求体内容的输入流
        ByteArrayInputStream byteArrayInputStream = new ByteArrayInputStream(requestBody.getBytes(StandardCharsets.UTF_8));

        // 将原始请求的输入流替换为新的输入流
        return new HttpServletRequestWrapper((HttpServletRequest) request) {
            @Override
            public ServletInputStream getInputStream() throws IOException {
                return new ServletInputStreamWrapper(byteArrayInputStream);
            }
        };
    }

    // 其他 Filter 接口的方法
}
```

在这个例子中，`readRequestBody` 方法用于读取请求体内容，而 `resetInputStream` 方法则用于将输入流重新设置为包含请求体内容的新输入流。这样，后续的过滤器就可以重新读取请求体而不会遇到流已关闭的问题。

请注意，这只是一个示例，具体实现可能需要根据你的应用程序的具体需求进行调整。