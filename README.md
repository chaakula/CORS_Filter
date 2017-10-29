# CORS_Filter

#Step 1: Filter java file

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

#Step 2: web.xml

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

	