# JSP, JavaServer Pages

* HTML 코드에 Java 코드를 넣어 동적 웹 페이지를 생성하는 웹 애플리케이션 도구
* JSP 가 실행되면 자바 서블릿으로 변환되며 웹 애플리케이션 서버에서 동작되며 필요한 기능을 수행한다
* 그렇게 생성된 데이터를 웹 페이지와 함께 클라이언트로 응답한다

> ### 웹
> * 인터넷 기반의 정보 기술
> * World Wide Web -> WWW
> * 전세계에 거대한 네트워크 망을 통해 정보 공유
> * 정보의 흐름은 양방향성


> ### 웹 애플리케이션, Web Application
> * 웹에서 실행되는 응용 프로그램
> * 인터넷을 통한 여러 서비스를 총칭한다
> * Request and Reply 로 구성된다
> * 웹 브라우저 : 클라이언트에서 요청하고 전달받은 페이지를 볼 수 있는 환경
> * 웹 서버 : 클라이언트로부터 요청받아 서버에 저장된 리소스를 클라이언트에게 전달한다
    >
* 주로 정적 컨텐츠
> * 웹 애플리케이션 서버 : 서버단에서 필요한 기능을 수행하고 그 결과를 웹 서버에 전달
    >
* WAS 라고도 부른다
> * 데이터베이스 : 서비스에 필요한 데이터를 보관, 갱신 등 관리를 한다

> ### 자바 서블릿, Java Servlet
> * 웹 페이지를 동적으로 생성하기 위한 서버측 프로그램
> * 자바 언어를 기반으로 만들어지며 웹 애플리케이션 서버 위에서 컴파일 되고 동작한다

> ### JSP and Servlet
> * 결과적으로 하는 일은 동일하다
> * JSP : 는 HTML 내부에 소스코드가 들어감으로 인해 HTML 코드를 작성하기 편하다
> * Servlet : 지바 코드 내에 HTML 코드가 있어서 읽고 쓰기가 굉장히 불편하기 때문에 작업의 효율성이 떨어진다
> * JSP 로 작성된 프로그램은 서버로 요청시 서블릿 파일로 변환되어 JSP 태그를 분해하고 추출하여 다시 순수한 HTML을 변환한다

<br>

# JSP 가 만들어진 이유

### 사용자가 원할 때마다 새로은 HTML 을 만들어줄 수는 없다

* 사용자가 보고 싶은 내용은 계속 바뀔 것이다
    * 사용자 정보, 상품 정보 등
* 그런데 HTML 은 정적이라서 정해진 것 밖에 보여줄 수 없다
* 하지만 JSP 를 사용하면 동적인 웹 애플리케이션을 만들어 사용자 참여가 늘어난다
* 업데이트 시간도 줄이고 관리 시간도 줄일 수 있다

<br>

# JSP 는 Servlet 기술의 확장형이라 볼 수 있다

* Servlet 을 보완한 스크립트 방식 표준
* Servlet 의 모든 기능 + 추가적인 기능을 가진다

### 추가적인 기능들

* Implicit Objects
* Predefined Tags
* Expression Language
* Custom Tags

<br>

# 코드 예싲

## Servlet

```java
/*
 * Generated by the Jasper component of Apache Tomcat
 * Version: Apache Tomcat/8.5.50
 * Generated at: 2020-05-26 04:57:04 UTC
 * Note: The last modified time of this file was set to
 *       the last modified time of the source file after
 *       generation to assist with modification tracking.
 */
package org.apache.jsp;

import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.jsp.*;

public final class jspCalCulator_jsp extends org.apache.jasper.runtime.HttpJspBase
        implements org.apache.jasper.runtime.JspSourceDependent,
        org.apache.jasper.runtime.JspSourceImports {

    private static final javax.servlet.jsp.JspFactory _jspxFactory =
            javax.servlet.jsp.JspFactory.getDefaultFactory();

    private static java.util.Map<java.lang.String, java.lang.Long> _jspx_dependants;

    private static final java.util.Set<java.lang.String> _jspx_imports_packages;

    private static final java.util.Set<java.lang.String> _jspx_imports_classes;

    static {
        _jspx_imports_packages = new java.util.HashSet<>();
        _jspx_imports_packages.add("javax.servlet");
        _jspx_imports_packages.add("javax.servlet.http");
        _jspx_imports_packages.add("javax.servlet.jsp");
        _jspx_imports_classes = null;
    }

    private volatile javax.el.ExpressionFactory _el_expressionfactory;
    private volatile org.apache.tomcat.InstanceManager _jsp_instancemanager;

    public java.util.Map<java.lang.String, java.lang.Long> getDependants() {
        return _jspx_dependants;
    }

    public java.util.Set<java.lang.String> getPackageImports() {
        return _jspx_imports_packages;
    }

    public java.util.Set<java.lang.String> getClassImports() {
        return _jspx_imports_classes;
    }

    public javax.el.ExpressionFactory _jsp_getExpressionFactory() {
        if (_el_expressionfactory == null) {
            synchronized (this) {
                if (_el_expressionfactory == null) {
                    _el_expressionfactory = _jspxFactory.getJspApplicationContext(getServletConfig().getServletContext()).getExpressionFactory();
                }
            }
        }
        return _el_expressionfactory;
    }

    public org.apache.tomcat.InstanceManager _jsp_getInstanceManager() {
        if (_jsp_instancemanager == null) {
            synchronized (this) {
                if (_jsp_instancemanager == null) {
                    _jsp_instancemanager = org.apache.jasper.runtime.InstanceManagerFactory.getInstanceManager(getServletConfig());
                }
            }
        }
        return _jsp_instancemanager;
    }

    public void _jspInit() {
    }

    public void _jspDestroy() {
    }

    public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response)
            throws java.io.IOException, javax.servlet.ServletException {

        final java.lang.String _jspx_method = request.getMethod();
        if (!"GET".equals(_jspx_method) && !"POST".equals(_jspx_method) && !"HEAD".equals(_jspx_method) && !javax.servlet.DispatcherType.ERROR.equals(request.getDispatcherType())) {
            response.sendError(HttpServletResponse.SC_METHOD_NOT_ALLOWED, "JSPs only permit GET POST or HEAD");
            return;
        }

        final javax.servlet.jsp.PageContext pageContext;
        javax.servlet.http.HttpSession session = null;
        final javax.servlet.ServletContext application;
        final javax.servlet.ServletConfig config;
        javax.servlet.jsp.JspWriter out = null;
        final java.lang.Object page = this;
        javax.servlet.jsp.JspWriter _jspx_out = null;
        javax.servlet.jsp.PageContext _jspx_page_context = null;


        try {
            response.setContentType("text/html; charset=UTF-8");
            pageContext = _jspxFactory.getPageContext(this, request, response,
                    null, true, 8192, true);
            _jspx_page_context = pageContext;
            application = pageContext.getServletContext();
            config = pageContext.getServletConfig();
            session = pageContext.getSession();
            out = pageContext.getOut();
            _jspx_out = out;

            out.write("\r\n");
            out.write("<!DOCTYPE html>\r\n");
            out.write("<html>\r\n");
            out.write("<head>\r\n");
            out.write("<meta charset=\"UTF-8\">\r\n");
            out.write("<title>Insert title here</title>\r\n");
            out.write("<style>\r\n");
            out.write("input{\r\n");
            out.write("\twidth:50px;\r\n");
            out.write("\theight:50px;\r\n");
            out.write("}\r\n");
            out.write(".output{\r\n");
            out.write("\theight: 50px;\r\n");
            out.write("\tbackground: #e9e9e9;\r\n");
            out.write("\tfont-size:24px;\r\n");
            out.write("\tfont-weight: bold;\r\n");
            out.write("\ttext-align: right;\r\n");
            out.write("\tpadding:0px 5px;\r\n");
            out.write("}\r\n");
            out.write("</style>\r\n");
            out.write("</head>\r\n");
            out.write("<body>\r\n");
            out.write("\r\n");
            out.write("\t<div>\r\n");
            out.write("\t\t<form action = \"calexample.jsp\" method=\"post\">\r\n");
            out.write("\t\t\t<table>\r\n");
            out.write("\t\t\t  <tr>\r\n");
            out.write("\t\t\t  \t<td class=\"output\" colspan=\"4\">");
            out.print(3 + 4);
            out.write("</td>\r\n");
            out.write("\t\t\t  </tr>\r\n");
            out.write("\t\t\t  <tr>\r\n");
            out.write("\t\t\t  \t<td><input type=\"submit\" name=\"operator\" value=\"CE\"/></td>\r\n");
            out.write("\t\t\t  \t<td><input type=\"submit\" name=\"operator\" value=\"C\"/></td>\r\n");
            out.write("\t\t\t  \t<td><input type=\"submit\" name=\"operator\" value=\"BS\"/></td>\r\n");
            out.write("\t\t\t  \t<td><input type=\"submit\" name=\"operator\" value=\"/\"/></td>\r\n");
            out.write("\t\t\t  </tr>\r\n");
            out.write("\t\t\t  <tr>\r\n");
            out.write("\t\t\t  \t<td><input type=\"submit\" name=\"value\" value=\"7\"/></td>\r\n");
            out.write("\t\t\t  \t<td><input type=\"submit\" name=\"value\" value=\"8\"/></td>\r\n");
            out.write("\t\t\t  \t<td><input type=\"submit\" name=\"value\" value=\"9\"/></td>\r\n");
            out.write("\t\t\t  \t<td><input type=\"submit\" name=\"operator\" value=\"*\"/></td>\r\n");
            out.write("\t\t\t  </tr>\r\n");
            out.write("\t\t\t  <tr>\r\n");
            out.write("\t\t\t  \t<td><input type=\"submit\" name=\"value\" value=\"4\"/></td>\r\n");
            out.write("\t\t\t  \t<td><input type=\"submit\" name=\"value\" value=\"5\"/></td>\r\n");
            out.write("\t\t\t  \t<td><input type=\"submit\" name=\"value\" value=\"6\"/></td>\r\n");
            out.write("\t\t\t  \t<td><input type=\"submit\" name=\"operator\" value=\"-\"/></td>\r\n");
            out.write("\t\t\t  </tr>\r\n");
            out.write("\t\t\t  <tr>\r\n");
            out.write("\t\t\t  \t<td><input type=\"submit\" name=\"value\" value=\"1\"/></td>\r\n");
            out.write("\t\t\t  \t<td><input type=\"submit\" name=\"value\" value=\"2\"/></td>\r\n");
            out.write("\t\t\t  \t<td><input type=\"submit\" name=\"value\" value=\"3\"/></td>\r\n");
            out.write("\t\t\t  \t<td><input type=\"submit\" name=\"operator\" value=\"+\"/></td>\r\n");
            out.write("\t\t\t  </tr>\r\n");
            out.write("\t\t\t  <tr>\r\n");
            out.write("\t\t\t  \t<td></td>\r\n");
            out.write("\t\t\t  \t<td><input type=\"submit\" name=\"value\" value=\"0\"/></td>\r\n");
            out.write("\t\t\t  \t<td><input type=\"submit\" name=\"dot\" value=\".\"/></td>\r\n");
            out.write("\t\t\t  \t<td><input type=\"submit\" name=\"operator\" value=\"=\"/></td>\r\n");
            out.write("\t\t\t  </tr>\r\n");
            out.write("\t\t\t</table>\t\t\r\n");
            out.write("\t\t\t\r\n");
            out.write("\t\t</form>\r\n");
            out.write("\t</div>\r\n");
            out.write("</body>\r\n");
            out.write("</html>");
        } catch (java.lang.Throwable t) {
            if (!(t instanceof javax.servlet.jsp.SkipPageException)) {
                out = _jspx_out;
                if (out != null && out.getBufferSize() != 0)
                    try {
                        if (response.isCommitted()) {
                            out.flush();
                        } else {
                            out.clearBuffer();
                        }
                    } catch (java.io.IOException e) {
                    }
                if (_jspx_page_context != null) _jspx_page_context.handlePageException(t);
                else throw new ServletException(t);
            }
        } finally {
            _jspxFactory.releasePageContext(_jspx_page_context);
        }
    }
}
```

## JSP

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>Insert title here</title>
</head>
<body>
<%
String user = request.getParameter("name");
if (user == null) {
user = "Guest";
}
%>

<%= user %>
</body>
</html>
```