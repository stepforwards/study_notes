---
title: java验证码 
tags: verifycode,验证码,Java
grammar_cjkRuby: true
---

# servlet书写

``` java
	package top.xiesen.testcodeblock;
    
    import com.sun.image.codec.jpeg.JPEGCodec;
    import com.sun.image.codec.jpeg.JPEGImageEncoder;
    
    import javax.servlet.ServletException;
    import javax.servlet.ServletOutputStream;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import javax.servlet.http.HttpSession;
    import java.awt.*;
    import java.awt.image.BufferedImage;
    import java.io.IOException;
    import java.util.Random;
    
    /**
     * Created by Allen on 2017/9/3.
     */
    public class VerifyCodeServlet extends HttpServlet {
        @Override
        protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            Random random=new Random();
            response.setContentType("image/jpeg");
            int width=70,height=20;
            BufferedImage bi=new BufferedImage(width,height,BufferedImage.TYPE_INT_RGB);
            Graphics g=bi.getGraphics();
            g.setColor(getRandColor(200, 255));
            g.fillRect(0, 0, width,height);
    
            g.setColor(getRandColor(160,200));
            for (int i=0;i<155;i++)
            {
                int x = random.nextInt(width);
                int y = random.nextInt(height);
                int xl = random.nextInt(12);
                int yl = random.nextInt(12);
                g.drawLine(x,y,x+xl,y+yl);
            }
            String strRandom="";
            String generateRandom="23456789abcdefghjkmnpqrstuvwxyzABCDEFGHJKLMNPQRSTUVWXYZ";
            for(int i=0;i<4;i++){
                int intRandom=random.nextInt(55);
                String numRandom=generateRandom.charAt(intRandom)+"";
                strRandom+=numRandom;
                g.setColor(new Color(20+random.nextInt(110),20+random.nextInt(110),20+random.nextInt(110)));
    
                g.setFont(new Font("Time NewRoman",Font.BOLD,14+random.nextInt(6)));
                g.drawString(numRandom,6+13*i, 16);
            }
    
            HttpSession session=request.getSession();
            session.setAttribute("strRandom", strRandom);
            g.dispose();
            ServletOutputStream sos=response.getOutputStream();
            JPEGImageEncoder encoder= JPEGCodec.createJPEGEncoder(sos);
            encoder.encode(bi);
            sos.close();
            bi=null;
    
        }
        /*设置随机颜色*/
        private Color getRandColor(int fc,int bc){
            Random random = new Random();
            if(fc>255) fc=255;
            if(bc>255) bc=255;
            int r=fc+random.nextInt(bc-fc);
            int g=fc+random.nextInt(bc-fc);
            int b=fc+random.nextInt(bc-fc);
            return new Color(r,g,b);
        }
    
        @Override
        protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            doGet(req,resp);
        }
    }

```

# Web.xml配置

``` xml
<servlet>
	<servlet-name>verifyCode</servlet-name>
	<servlet-class>top.xiesen.testcodeblock.VerifyCodeServlet</servlet-class>
</servlet>
<servlet-mapping>
	<servlet-name>verifyCode</servlet-name>
	<url-pattern>/verifycode</url-pattern>
</servlet-mapping>
```

# 使用教程

``` html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>验证码测试</title>
    <script type=text/javascript>
        var i=0;
        /*点击图片换一张*/
        function changeImg(img)
        {
            img.src="<%=request.getContextPath()%>/verifycode?i="+i;
            i++;
        }
        /*点击a标签换一张*/
        function huanyizhang() {
            document.getElementById("validcode").src = "<%=request.getContextPath()%>/verifycode?i="+i;
            i++;
        }
    </script>
  </head>
  <body>
  <img style="cursor:hand;border:none;}" id="validcode" name="validcode"
       src="<%=request.getContextPath()%>/verifycode" align="absmiddle" onclick="changeImg(this)"/>
  <a onclick="huanyizhang()" href="#">换一张</a>
  </body>
</html>
```


