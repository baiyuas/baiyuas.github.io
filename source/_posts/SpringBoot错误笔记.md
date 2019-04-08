
# 问题1

HTTP Status 406 – Not Acceptable
Type Status Report

Description The target resource does not have a current representation that would be acceptable to the user agent, according to the proactive negotiation header fields received in the request, and the server is unwilling to supply a default representation.

Apache Tomcat/8.5.35

**解决**

1. 排查是否请求后缀使用了.html或者.htm
2. 缺少jackson的jar包，添加依赖

我的错误：使用Intelliji部署到啊Tomcat，启动后编译器没有将后添加的jackson包输出到target目录下，所以总是缺少jar包

SpringMVC默认配置了如下集中MessageConvert，如果你使用其中某个，需要添加对应的jar文件

		romePresent = ClassUtils.isPresent("com.rometools.rome.feed.WireFeed", classLoader);
		jaxb2Present = ClassUtils.isPresent("javax.xml.bind.Binder", classLoader);
		jackson2Present = ClassUtils.isPresent("com.fasterxml.jackson.databind.ObjectMapper", classLoader) &&
						ClassUtils.isPresent("com.fasterxml.jackson.core.JsonGenerator", classLoader);
		jackson2XmlPresent = ClassUtils.isPresent("com.fasterxml.jackson.dataformat.xml.XmlMapper", classLoader);
		jackson2SmilePresent = ClassUtils.isPresent("com.fasterxml.jackson.dataformat.smile.SmileFactory", classLoader);
		jackson2CborPresent = ClassUtils.isPresent("com.fasterxml.jackson.dataformat.cbor.CBORFactory", classLoader);
		gsonPresent = ClassUtils.isPresent("com.google.gson.Gson", classLoader);
		jsonbPresent = ClassUtils.isPresent("javax.json.bind.Jsonb", classLoader);

# 问题2

错误描述

	Maven:Failed to read artifact descriptor for xxx

Maven多模块项目jar包引用问题，先将父pom.xml进行打包发布，然后在对子module打包发布，之后在引用就不会报错了

参考[https://www.cnblogs.com/wpbxin/p/9715114.html](https://www.cnblogs.com/wpbxin/p/9715114.html)

