# CORS_Filter

## Step 1: Filter java file
```java
import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class CORSFilter implements Filter {

	@Override
	public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws IOException,
			ServletException {
		HttpServletResponse httpResp = (HttpServletResponse) resp;
		HttpServletRequest httpReq = (HttpServletRequest) req;

		httpResp.setHeader("Access-Control-Allow-Methods", "GET, POST, PUT, OPTIONS");
		httpResp.setHeader("Access-Control-Allow-Origin", "*");
		if (httpReq.getMethod().equalsIgnoreCase("OPTIONS")) {
			httpResp.setHeader("Access-Control-Allow-Headers",
					httpReq.getHeader("Access-Control-Request-Headers"));
		}
		chain.doFilter(req, resp);
	}

	@Override
	public void init(FilterConfig arg0) throws ServletException {
	}

	@Override
	public void destroy() {
	}
}
```
## Step 2: web.xml
```xml
<?xml version="1.0" encoding="utf-8" standalone="no"?>
	<web-app xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" version="2.5"
		xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">

		<welcome-file-list>
			<welcome-file>index.html</welcome-file>
		</welcome-file-list>
		<filter>
			<filter-name>cors_filter</filter-name>
			<filter-class>com.df.angularfileupload.CORSFilter</filter-class>
		</filter>
		<filter-mapping>
			<filter-name>cors_filter</filter-name>
			<url-pattern>*</url-pattern>
		</filter-mapping>
</web-app>
```
	
# CORS Filter in restful Web services

```java
import org.springframework.core.Ordered;
import org.springframework.core.annotation.Order;
import org.springframework.stereotype.Component;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;


@Component
@Order(Ordered.HIGHEST_PRECEDENCE)
public class CorsFilter implements Filter {

    @Override
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain) throws IOException, ServletException {
        final HttpServletResponse response = (HttpServletResponse) res;
        response.setHeader("Access-Control-Allow-Origin", "*");
        response.setHeader("Access-Control-Allow-Methods", "POST, PUT, GET, OPTIONS, DELETE");
        response.setHeader("Access-Control-Allow-Headers", "Authorization, Content-Type");
        response.setHeader("Access-Control-Max-Age", "3600");
        if ("OPTIONS".equalsIgnoreCase(((HttpServletRequest) req).getMethod())) {
            response.setStatus(HttpServletResponse.SC_OK);
        } else {
            chain.doFilter(req, res);
        }
    }

    @Override
    public void destroy() {
    }

    @Override
    public void init(FilterConfig config) throws ServletException {

    }
}
```

# CORS Filter in jaxrs Web services

```java
import java.io.IOException;

import javax.ws.rs.container.ContainerRequestContext;
import javax.ws.rs.container.ContainerResponseContext;
import javax.ws.rs.container.ContainerResponseFilter;
import javax.ws.rs.ext.Provider;

@Provider
public class CORSFilter implements ContainerResponseFilter {

    @Override
    public void filter(final ContainerRequestContext requestContext,
                       final ContainerResponseContext cres) throws IOException {
        cres.getHeaders().add("Access-Control-Allow-Origin", "*");
        cres.getHeaders().add("Access-Control-Allow-Credentials", "true");
        cres.getHeaders().add("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE, OPTIONS, HEAD");
        cres.getHeaders().add("Access-Control-Max-Age", "1209600");
    }

}
```
