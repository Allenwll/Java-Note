# WEB 漏洞防护

## 1. web.xml

#### 1. 启用CSP
添加 http头 X-XSS-Protection X-Frame-Option，添加资源CSP限制
```xml
<filter>
	<filter-name>xxssProtection</filter-name>
	<filter-class>com.wisdombud.cqupt.edu.web.filter.XXSSProtectionFilter</filter-class>

	<init-param>
		<!-- If not specified the default is false -->
		<param-name>report-only</param-name>
		<param-value>false</param-value>
	</init-param>
	<!-- Optionally add a reporter-uri -->

	<init-param>
		<param-name>sandbox</param-name>
		<param-value>allow-forms allow-same-origin allow-scripts allow-popups allow-pointer-lock
			allow-popups-to-escape-sandbox allow-top-navigation allow-orientation-lock
		</param-value>
		<!-- true enables the sandbox behaviour - the default is false - one can also specify exceptions, e.g.
		<param-value>allow-forms allow-same-origin</param-value>
		-->
	</init-param>
	<!-- Remember that special keywords have to be put in single quotes, e.g. 'none', 'self' -->
	<init-param>
		<!-- If not specified the default is 'none' -->
		<param-name>default-src</param-name>
		<param-value>*</param-value>
	</init-param>
	<init-param>
		<param-name>img-src</param-name>
		<param-value>'self' data:</param-value>
	</init-param>
	<init-param>
		<param-name>script-src</param-name>
		<!--<param-value>'self' job.cqupt.edu.cn</param-value>-->
		<param-value>'self' 'unsafe-inline' 'unsafe-eval'</param-value>
	</init-param>
	<init-param>
		<param-name>style-src</param-name>
		<param-value>'self' 'unsafe-inline'</param-value>
	</init-param>
	<init-param>
		<param-name>connect-src</param-name>
		<param-value>'self'</param-value>
	</init-param>
	<init-param>
		<param-name>font-src</param-name>
		<param-value>'self'</param-value>
	</init-param>
	<init-param>
		<param-name>object-src</param-name>
		<param-value>'self'</param-value>
	</init-param>
	<init-param>
		<param-name>media-src</param-name>
		<param-value>'self'</param-value>
	</init-param>
	<init-param>
		<param-name>frame-src</param-name>
		<param-value>'self'</param-value>
	</init-param>
</filter>
<filter-mapping>
	<filter-name>xxssProtection</filter-name>
	<url-pattern>*.html</url-pattern>
</filter-mapping>
```
 
#### 2. 启用cookie httponly
 
```xml
<session-config>
	<tracking-mode>COOKIE</tracking-mode>
	<cookie-config>
		<http-only>true</http-only>
	</cookie-config>
</session-config>
```

#### 3. 错误页面

```xml
<error-page> 
    <error-code>404</error-code>
    <location>/404.html</location>
</error-page>
<error-page> 
    <error-code>500</error-code>
    <location>/500.html</location>
</error-page>

```


## 2. JSP 

#### 1. 直接 ognl 取值

- java:

```java
this.title = ESAPI.encoder().encodeForJavaScript(this.title);
return "search"
```


- JS:引入JS
```xml
<script src="${contextPath}/resources/3rd-js/esapi/resources/i18n/ESAPI_Standard_en_US.properties.js?ver=<%=version%>"></script>
<script src="${contextPath}/resources/3rd-js/esapi/esapi-compressed.js?ver=<%=version%>"></script>
<script src="${contextPath}/resources/3rd-js/esapi/resources/Base.esapi.properties.js?ver=<%=version%>"></script>
<script src="${contextPath}/resources/3rd-js/esapi/esapi.js?ver=<%=version%>"></script>

```

初始化：
```xml
<script type="text/javascript">
    Base.esapi.properties.logging['ApplicationLogger'] = {
        Level: org.owasp.esapi.Logger.ALL,
        Appenders: [new Log4js.ConsoleAppender()],
        LogUrl: true,
        LogApplicationName: true,
        EncodingRequired: true
    };

    Base.esapi.properties.application.Name = "v1.0";
    // Initialize the api
    org.owasp.esapi.ESAPI.initialize();
</script>

```

使用：
```js
 title = $ESAPI.encoder().encodeForJavaScript('${title}');
 ```



#### 2 关闭form autocomplte
为了方式浏览器自动记录form内容

## 3. java

#### 1. 登录，清除old session，生成new session


```java

public void doGet(HttpServletRequest request, HttpServletResponse response)
     throws ServletException, IOException {
     if(userid.equals("admin") && password.equals("admin"))  {
        request.getSession().invalidate();
        HttpSession session = request.getSession(true);
        session.setAttribute("AUTHENTICATED", new Boolean(true));
        response.sendRedirect("PageRequiringAuthentication.jsp");
	}
}
```        

#### 2. 启用token验证

```
对于新增，修改，必须得先在java端生成token，然后在打开jsp页面时获得token，最后在提交正常参数以外也需要提交token，服务器端验证token是否存在，并且未超时，则执行业务代码。

同时需要定时删除超时token

```


#### 3. 文件上传
使用regex限定上传路径，且上传路径在tomcat路径之外



#### 4. action 验证 菜单权限
在action中验证访问权限，无权限返回无权限错误代码或无权限页面




## 4. struts2

#### 1. 禁用debugmode

修改struts.xml:
```
struts.devMode=false
```



## 5. Tomcat

#### 1. 隐藏版本

```
进入tomcat lib目录：
cd /home/tomcat/lib

创建文件夹 org/apache/catalina/util
mkdir -p org/apache/catalina/util

新建ServerInfo.properties
vi ServerInfo.properties

内容如下：
server.info=Apache Tomcat Version X
```

#### 2.隐藏tomcat Apache-Coyote 版本号

tomcat/conf/server.xml
connector节点添加server属性，例如：

```xml
<Connector port="8080" protocol="HTTP/1.1" server="Apache">
```



#### 3. 关闭自动部署

server.xml host节点

unpackWARs="false" autoDeploy="false"


### 备注
esapi maven依赖
```xml
<dependency>
	<groupId>org.owasp.esapi</groupId>
	<artifactId>esapi</artifactId>
	<version>2.1.0.1</version>
</dependency>
```


ESAPI.properties,将ESAPI.properties 添加到 classpath

```properties
#
# OWASP Enterprise Security API (ESAPI) Properties file -- PRODUCTION Version
#
# This file is part of the Open Web Application Security Project (OWASP)
# Enterprise Security API (ESAPI) project. For details, please see
# http://www.owasp.org/index.php/ESAPI.
#
# Copyright (c) 2008,2009 - The OWASP Foundation
#
# DISCUSS: This may cause a major backwards compatibility issue, etc. but
#		   from a name space perspective, we probably should have prefaced
#		   all the property names with ESAPI or at least OWASP. Otherwise
#		   there could be problems is someone loads this properties file into
#		   the System properties.  We could also put this file into the
#		   esapi.jar file (perhaps as a ResourceBundle) and then allow an external
#		   ESAPI properties be defined that would overwrite these defaults.
#		   That keeps the application's properties relatively simple as usually
#		   they will only want to override a few properties. If looks like we
#		   already support multiple override levels of this in the
#		   DefaultSecurityConfiguration class, but I'm suggesting placing the
#		   defaults in the esapi.jar itself. That way, if the jar is signed,
#		   we could detect if those properties had been tampered with. (The
#		   code to check the jar signatures is pretty simple... maybe 70-90 LOC,
#		   but off course there is an execution penalty (similar to the way
#		   that the separate sunjce.jar used to be when a class from it was
#		   first loaded). Thoughts?
###############################################################################
#
# WARNING: Operating system protection should be used to lock down the .esapi
# resources directory and all the files inside and all the directories all the
# way up to the root directory of the file system.  Note that if you are using
# file-based implementations, that some files may need to be read-write as they
# get updated dynamically.
#
# Before using, be sure to update the MasterKey and MasterSalt as described below.
# N.B.: If you had stored data that you have previously encrypted with ESAPI 1.4,
#		you *must* FIRST decrypt it using ESAPI 1.4 and then (if so desired)
#		re-encrypt it with ESAPI 2.0. If you fail to do this, you will NOT be
#		able to decrypt your data with ESAPI 2.0.
#
#		YOU HAVE BEEN WARNED!!! More details are in the ESAPI 2.0 Release Notes.
#
#===========================================================================
# ESAPI Configuration
#
# If true, then print all the ESAPI properties set here when they are loaded.
# If false, they are not printed. Useful to reduce output when running JUnit tests.
# If you need to troubleshoot a properties related problem, turning this on may help.
# This is 'false' in the src/test/resources/.esapi version. It is 'true' by
# default for reasons of backward compatibility with earlier ESAPI versions.
ESAPI.printProperties=true
# ESAPI is designed to be easily extensible. You can use the reference implementation
# or implement your own providers to take advantage of your enterprise's security
# infrastructure. The functions in ESAPI are referenced using the ESAPI locator, like:
#
#    String ciphertext =
#		ESAPI.encryptor().encrypt("Secret message");   // Deprecated in 2.0
#    CipherText cipherText =
#		ESAPI.encryptor().encrypt(new PlainText("Secret message")); // Preferred
#
# Below you can specify the classname for the provider that you wish to use in your
# application. The only requirement is that it implement the appropriate ESAPI interface.
# This allows you to switch security implementations in the future without rewriting the
# entire application.
#
# ExperimentalAccessController requires ESAPI-AccessControlPolicy.xml in .esapi directory
ESAPI.AccessControl=org.owasp.esapi.reference.DefaultAccessController
# FileBasedAuthenticator requires users.txt file in .esapi directory
ESAPI.Authenticator=org.owasp.esapi.reference.FileBasedAuthenticator
ESAPI.Encoder=org.owasp.esapi.reference.DefaultEncoder
ESAPI.Encryptor=org.owasp.esapi.reference.crypto.JavaEncryptor
ESAPI.Executor=org.owasp.esapi.reference.DefaultExecutor
ESAPI.HTTPUtilities=org.owasp.esapi.reference.DefaultHTTPUtilities
ESAPI.IntrusionDetector=org.owasp.esapi.reference.DefaultIntrusionDetector
# Log4JFactory Requires log4j.xml or log4j.properties in classpath - http://www.laliluna.de/log4j-tutorial.html
ESAPI.Logger=org.owasp.esapi.reference.Log4JLogFactory
#ESAPI.Logger=org.owasp.esapi.reference.JavaLogFactory
ESAPI.Randomizer=org.owasp.esapi.reference.DefaultRandomizer
ESAPI.Validator=org.owasp.esapi.reference.DefaultValidator
#===========================================================================
# ESAPI Authenticator
#
Authenticator.AllowedLoginAttempts=3
Authenticator.MaxOldPasswordHashes=13
Authenticator.UsernameParameterName=username
Authenticator.PasswordParameterName=password
# RememberTokenDuration (in days)
Authenticator.RememberTokenDuration=14
# Session Timeouts (in minutes)
Authenticator.IdleTimeoutDuration=20
Authenticator.AbsoluteTimeoutDuration=120
#===========================================================================
# ESAPI Encoder
#
# ESAPI canonicalizes input before validation to prevent bypassing filters with encoded attacks.
# Failure to canonicalize input is a very common mistake when implementing validation schemes.
# Canonicalization is automatic when using the ESAPI Validator, but you can also use the
# following code to canonicalize data.
#
#      ESAPI.Encoder().canonicalize( "%22hello world&#x22;" );
#
# Multiple encoding is when a single encoding format is applied multiple times. Allowing
# multiple encoding is strongly discouraged.
Encoder.AllowMultipleEncoding=false
# Mixed encoding is when multiple different encoding formats are applied, or when
# multiple formats are nested. Allowing multiple encoding is strongly discouraged.
Encoder.AllowMixedEncoding=false
# The default list of codecs to apply when canonicalizing untrusted data. The list should include the codecs
# for all downstream interpreters or decoders. For example, if the data is likely to end up in a URL, HTML, or
# inside JavaScript, then the list of codecs below is appropriate. The order of the list is not terribly important.
Encoder.DefaultCodecList=HTMLEntityCodec,PercentCodec,JavaScriptCodec
#===========================================================================
# ESAPI Encryption
#
# The ESAPI Encryptor provides basic cryptographic functions with a simplified API.
# To get started, generate a new key using java -classpath esapi.jar org.owasp.esapi.reference.crypto.JavaEncryptor
# There is not currently any support for key rotation, so be careful when changing your key and salt as it
# will invalidate all signed, encrypted, and hashed data.
#
# WARNING: Not all combinations of algorithms and key lengths are supported.
# If you choose to use a key length greater than 128, you MUST download the
# unlimited strength policy files and install in the lib directory of your JRE/JDK.
# See http://java.sun.com/javase/downloads/index.jsp for more information.
#
# Backward compatibility with ESAPI Java 1.4 is supported by the two deprecated API
# methods, Encryptor.encrypt(String) and Encryptor.decrypt(String). However, whenever
# possible, these methods should be avoided as they use ECB cipher mode, which in almost
# all circumstances a poor choice because of it's weakness. CBC cipher mode is the default
# for the new Encryptor encrypt / decrypt methods for ESAPI Java 2.0.  In general, you
# should only use this compatibility setting if you have persistent data encrypted with
# version 1.4 and even then, you should ONLY set this compatibility mode UNTIL
# you have decrypted all of your old encrypted data and then re-encrypted it with
# ESAPI 2.0 using CBC mode. If you have some reason to mix the deprecated 1.4 mode
# with the new 2.0 methods, make sure that you use the same cipher algorithm for both
# (256-bit AES was the default for 1.4; 128-bit is the default for 2.0; see below for
# more details.) Otherwise, you will have to use the new 2.0 encrypt / decrypt methods
# where you can specify a SecretKey. (Note that if you are using the 256-bit AES,
# that requires downloading the special jurisdiction policy files mentioned above.)
#
#		***** IMPORTANT: Do NOT forget to replace these with your own values! *****
# To calculate these values, you can run:
#		java -classpath esapi.jar org.owasp.esapi.reference.crypto.JavaEncryptor
#
Encryptor.MasterKey=tzfztf56ftv
Encryptor.MasterSalt=123456ztrewq
# Provides the default JCE provider that ESAPI will "prefer" for its symmetric
# encryption and hashing. (That is it will look to this provider first, but it
# will defer to other providers if the requested algorithm is not implemented
# by this provider.) If left unset, ESAPI will just use your Java VM's current
# preferred JCE provider, which is generally set in the file
# "$JAVA_HOME/jre/lib/security/java.security".
#
# The main intent of this is to allow ESAPI symmetric encryption to be
# used with a FIPS 140-2 compliant crypto-module. For details, see the section
# "Using ESAPI Symmetric Encryption with FIPS 140-2 Cryptographic Modules" in
# the ESAPI 2.0 Symmetric Encryption User Guide, at:
# http://owasp-esapi-java.googlecode.com/svn/trunk/documentation/esapi4java-core-2.0-symmetric-crypto-user-guide.html
# However, this property also allows you to easily use an alternate JCE provider
# such as "Bouncy Castle" without having to make changes to "java.security".
# See Javadoc for SecurityProviderLoader for further details. If you wish to use
# a provider that is not known to SecurityProviderLoader, you may specify the
# fully-qualified class name of the JCE provider class that implements
# java.security.Provider. If the name contains a '.', this is interpreted as
# a fully-qualified class name that implements java.security.Provider.
#
# NOTE: Setting this property has the side-effect of changing it in your application
#       as well, so if you are using JCE in your application directly rather than
#       through ESAPI (you wouldn't do that, would you? ;-), it will change the
#       preferred JCE provider there as well.
#
# Default: Keeps the JCE provider set to whatever JVM sets it to.
Encryptor.PreferredJCEProvider=
# AES is the most widely used and strongest encryption algorithm. This
# should agree with your Encryptor.CipherTransformation property.
# By default, ESAPI Java 1.4 uses "PBEWithMD5AndDES" and which is
# very weak. It is essentially a password-based encryption key, hashed
# with MD5 around 1K times and then encrypted with the weak DES algorithm
# (56-bits) using ECB mode and an unspecified padding (it is
# JCE provider specific, but most likely "NoPadding"). However, 2.0 uses
# "AES/CBC/PKCSPadding". If you want to change these, change them here.
# Warning: This property does not control the default reference implementation for
#		   ESAPI 2.0 using JavaEncryptor. Also, this property will be dropped
#		   in the future.
# @deprecated
Encryptor.EncryptionAlgorithm=AES
#		For ESAPI Java 2.0 - New encrypt / decrypt methods use this.
Encryptor.CipherTransformation=AES/CBC/PKCS5Padding
# Applies to ESAPI 2.0 and later only!
# Comma-separated list of cipher modes that provide *BOTH*
# confidentiality *AND* message authenticity. (NIST refers to such cipher
# modes as "combined modes" so that's what we shall call them.) If any of these
# cipher modes are used then no MAC is calculated and stored
# in the CipherText upon encryption. Likewise, if one of these
# cipher modes is used with decryption, no attempt will be made
# to validate the MAC contained in the CipherText object regardless
# of whether it contains one or not. Since the expectation is that
# these cipher modes support support message authenticity already,
# injecting a MAC in the CipherText object would be at best redundant.
#
# Note that as of JDK 1.5, the SunJCE provider does not support *any*
# of these cipher modes. Of these listed, only GCM and CCM are currently
# NIST approved. YMMV for other JCE providers. E.g., Bouncy Castle supports
# GCM and CCM with "NoPadding" mode, but not with "PKCS5Padding" or other
# padding modes.
Encryptor.cipher_modes.combined_modes=GCM,CCM,IAPM,EAX,OCB,CWC
# Applies to ESAPI 2.0 and later only!
# Additional cipher modes allowed for ESAPI 2.0 encryption. These
# cipher modes are in _addition_ to those specified by the property
# 'Encryptor.cipher_modes.combined_modes'.
# Note: We will add support for streaming modes like CFB & OFB once
# we add support for 'specified' to the property 'Encryptor.ChooseIVMethod'
# (probably in ESAPI 2.1).
# DISCUSS: Better name?
Encryptor.cipher_modes.additional_allowed=CBC
# 128-bit is almost always sufficient and appears to be more resistant to
# related key attacks than is 256-bit AES. Use '_' to use default key size
# for cipher algorithms (where it makes sense because the algorithm supports
# a variable key size). Key length must agree to what's provided as the
# cipher transformation, otherwise this will be ignored after logging a
# warning.
#
# NOTE: This is what applies BOTH ESAPI 1.4 and 2.0. See warning above about mixing!
Encryptor.EncryptionKeyLength=128
# Because 2.0 uses CBC mode by default, it requires an initialization vector (IV).
# (All cipher modes except ECB require an IV.) There are two choices: we can either
# use a fixed IV known to both parties or allow ESAPI to choose a random IV. While
# the IV does not need to be hidden from adversaries, it is important that the
# adversary not be allowed to choose it. Also, random IVs are generally much more
# secure than fixed IVs. (In fact, it is essential that feed-back cipher modes
# such as CFB and OFB use a different IV for each encryption with a given key so
# in such cases, random IVs are much preferred. By default, ESAPI 2.0 uses random
# IVs. If you wish to use 'fixed' IVs, set 'Encryptor.ChooseIVMethod=fixed' and
# uncomment the Encryptor.fixedIV.
#
# Valid values:		random|fixed|specified		'specified' not yet implemented; planned for 2.1
Encryptor.ChooseIVMethod=random
# If you choose to use a fixed IV, then you must place a fixed IV here that
# is known to all others who are sharing your secret key. The format should
# be a hex string that is the same length as the cipher block size for the
# cipher algorithm that you are using. The following is an *example* for AES
# from an AES test vector for AES-128/CBC as described in:
# NIST Special Publication 800-38A (2001 Edition)
# "Recommendation for Block Cipher Modes of Operation".
# (Note that the block size for AES is 16 bytes == 128 bits.)
#
Encryptor.fixedIV=0x000102030405060708090a0b0c0d0e0f
# Whether or not CipherText should use a message authentication code (MAC) with it.
# This prevents an adversary from altering the IV as well as allowing a more
# fool-proof way of determining the decryption failed because of an incorrect
# key being supplied. This refers to the "separate" MAC calculated and stored
# in CipherText, not part of any MAC that is calculated as a result of a
# "combined mode" cipher mode.
#
# If you are using ESAPI with a FIPS 140-2 cryptographic module, you *must* also
# set this property to false.
Encryptor.CipherText.useMAC=true
# Whether or not the PlainText object may be overwritten and then marked
# eligible for garbage collection. If not set, this is still treated as 'true'.
Encryptor.PlainText.overwrite=true
# Do not use DES except in a legacy situations. 56-bit is way too small key size.
#Encryptor.EncryptionKeyLength=56
#Encryptor.EncryptionAlgorithm=DES
# TripleDES is considered strong enough for most purposes.
#	Note:	There is also a 112-bit version of DESede. Using the 168-bit version
#			requires downloading the special jurisdiction policy from Sun.
#Encryptor.EncryptionKeyLength=168
#Encryptor.EncryptionAlgorithm=DESede
Encryptor.HashAlgorithm=SHA-512
Encryptor.HashIterations=1024
Encryptor.DigitalSignatureAlgorithm=SHA1withDSA
Encryptor.DigitalSignatureKeyLength=1024
Encryptor.RandomAlgorithm=SHA1PRNG
Encryptor.CharacterEncoding=UTF-8
# This is the Pseudo Random Function (PRF) that ESAPI's Key Derivation Function
# (KDF) normally uses. Note this is *only* the PRF used for ESAPI's KDF and
# *not* what is used for ESAPI's MAC. (Currently, HmacSHA1 is always used for
# the MAC, mostly to keep the overall size at a minimum.)
#
# Currently supported choices for JDK 1.5 and 1.6 are:
#	HmacSHA1 (160 bits), HmacSHA256 (256 bits), HmacSHA384 (384 bits), and
#	HmacSHA512 (512 bits).
# Note that HmacMD5 is *not* supported for the PRF used by the KDF even though
# the JDKs support it.  See the ESAPI 2.0 Symmetric Encryption User Guide
# further details.
Encryptor.KDF.PRF=HmacSHA256
#===========================================================================
# ESAPI HttpUtilties
#
# The HttpUtilities provide basic protections to HTTP requests and responses. Primarily these methods
# protect against malicious data from attackers, such as unprintable characters, escaped characters,
# and other simple attacks. The HttpUtilities also provides utility methods for dealing with cookies,
# headers, and CSRF tokens.
#
# Default file upload location (remember to escape backslashes with \\)
HttpUtilities.UploadDir=C:\\ESAPI\\testUpload
HttpUtilities.UploadTempDir=C:\\temp
# Force flags on cookies, if you use HttpUtilities to set cookies
HttpUtilities.ForceHttpOnlySession=false
HttpUtilities.ForceSecureSession=false
HttpUtilities.ForceHttpOnlyCookies=true
HttpUtilities.ForceSecureCookies=true
# Maximum size of HTTP headers
HttpUtilities.MaxHeaderSize=4096
# File upload configuration
HttpUtilities.ApprovedUploadExtensions=.zip,.pdf,.doc,.docx,.ppt,.pptx,.tar,.gz,.tgz,.rar,.war,.jar,.ear,.xls,.rtf,.properties,.java,.class,.txt,.xml,.jsp,.jsf,.exe,.dll
HttpUtilities.MaxUploadFileBytes=500000000
# Using UTF-8 throughout your stack is highly recommended. That includes your database driver,
# container, and any other technologies you may be using. Failure to do this may expose you
# to Unicode transcoding injection attacks. Use of UTF-8 does not hinder internationalization.
HttpUtilities.ResponseContentType=text/html; charset=UTF-8
# This is the name of the cookie used to represent the HTTP session
# Typically this will be the default "JSESSIONID"
HttpUtilities.HttpSessionIdName=JSESSIONID
#===========================================================================
# ESAPI Executor
# CHECKME - Not sure what this is used for, but surely it should be made OS independent.
Executor.WorkingDirectory=C:\\Windows\\Temp
Executor.ApprovedExecutables=C:\\Windows\\System32\\cmd.exe,C:\\Windows\\System32\\runas.exe
#===========================================================================
# ESAPI Logging
# Set the application name if these logs are combined with other applications
Logger.ApplicationName=ExampleApplication
# If you use an HTML log viewer that does not properly HTML escape log data, you can set LogEncodingRequired to true
Logger.LogEncodingRequired=false
# Determines whether ESAPI should log the application name. This might be clutter in some single-server/single-app environments.
Logger.LogApplicationName=true
# Determines whether ESAPI should log the server IP and port. This might be clutter in some single-server environments.
Logger.LogServerIP=true
# LogFileName, the name of the logging file. Provide a full directory path (e.g., C:\\ESAPI\\ESAPI_logging_file) if you
# want to place it in a specific directory.
Logger.LogFileName=ESAPI_logging_file
# MaxLogFileSize, the max size (in bytes) of a single log file before it cuts over to a new one (default is 10,000,000)
Logger.MaxLogFileSize=10000000
#===========================================================================
# ESAPI Intrusion Detection
#
# Each event has a base to which .count, .interval, and .action are added
# The IntrusionException will fire if we receive "count" events within "interval" seconds
# The IntrusionDetector is configurable to take the following actions: log, logout, and disable
#  (multiple actions separated by commas are allowed e.g. event.test.actions=log,disable
#
# Custom Events
# Names must start with "event." as the base
# Use IntrusionDetector.addEvent( "test" ) in your code to trigger "event.test" here
# You can also disable intrusion detection completely by changing
# the following parameter to true
#
IntrusionDetector.Disable=false
#
IntrusionDetector.event.test.count=2
IntrusionDetector.event.test.interval=10
IntrusionDetector.event.test.actions=disable,log
# Exception Events
# All EnterpriseSecurityExceptions are registered automatically
# Call IntrusionDetector.getInstance().addException(e) for Exceptions that do not extend EnterpriseSecurityException
# Use the fully qualified classname of the exception as the base
# any intrusion is an attack
IntrusionDetector.org.owasp.esapi.errors.IntrusionException.count=1
IntrusionDetector.org.owasp.esapi.errors.IntrusionException.interval=1
IntrusionDetector.org.owasp.esapi.errors.IntrusionException.actions=log,disable,logout
# for test purposes
# CHECKME: Shouldn't there be something in the property name itself that designates
#		   that these are for testing???
IntrusionDetector.org.owasp.esapi.errors.IntegrityException.count=10
IntrusionDetector.org.owasp.esapi.errors.IntegrityException.interval=5
IntrusionDetector.org.owasp.esapi.errors.IntegrityException.actions=log,disable,logout
# rapid validation errors indicate scans or attacks in progress
# org.owasp.esapi.errors.ValidationException.count=10
# org.owasp.esapi.errors.ValidationException.interval=10
# org.owasp.esapi.errors.ValidationException.actions=log,logout
# sessions jumping between hosts indicates session hijacking
IntrusionDetector.org.owasp.esapi.errors.AuthenticationHostException.count=2
IntrusionDetector.org.owasp.esapi.errors.AuthenticationHostException.interval=10
IntrusionDetector.org.owasp.esapi.errors.AuthenticationHostException.actions=log,logout
#===========================================================================
# ESAPI Validation
#
# The ESAPI Validator works on regular expressions with defined names. You can define names
# either here, or you may define application specific patterns in a separate file defined below.
# This allows enterprises to specify both organizational standards as well as application specific
# validation rules.
#
Validator.ConfigurationFile=validation.properties
# Validators used by ESAPI
Validator.AccountName=^[a-zA-Z0-9]{3,20}$
Validator.SystemCommand=^[a-zA-Z\\-\\/]{1,64}$
Validator.RoleName=^[a-z]{1,20}$
#the word TEST below should be changed to your application
#name - only relative URL's are supported
Validator.Redirect=^\\/test.*$
# Global HTTP Validation Rules
# Values with Base64 encoded data (e.g. encrypted state) will need at least [a-zA-Z0-9\/+=]
Validator.HTTPScheme=^(http|https)$
Validator.HTTPServerName=^[a-zA-Z0-9_.\\-]*$
Validator.HTTPParameterName=^[a-zA-Z0-9_]{1,32}$
Validator.HTTPParameterValue=^[a-zA-Z0-9.\\-\\/+=@_ ]*$
Validator.HTTPCookieName=^[a-zA-Z0-9\\-_]{1,32}$
Validator.HTTPCookieValue=^[a-zA-Z0-9\\-\\/+=_ ]*$
Validator.HTTPHeaderName=^[a-zA-Z0-9\\-_]{1,32}$
Validator.HTTPHeaderValue=^[a-zA-Z0-9()\\-=\\*\\.\\?;,+\\/:&_ ]*$
Validator.HTTPContextPath=^\\/?[a-zA-Z0-9.\\-\\/_]*$
Validator.HTTPServletPath=^[a-zA-Z0-9.\\-\\/_]*$
Validator.HTTPPath=^[a-zA-Z0-9.\\-_]*$
Validator.HTTPQueryString=^[a-zA-Z0-9()\\-=\\*\\.\\?;,+\\/:&_ %]*$
Validator.HTTPURI=^[a-zA-Z0-9()\\-=\\*\\.\\?;,+\\/:&_ ]*$
Validator.HTTPURL=^.*$
Validator.HTTPJSESSIONID=^[A-Z0-9]{10,30}$
# Validation of file related input
Validator.FileName=^[a-zA-Z0-9!@#$%^&{}\\[\\]()_+\\-=,.~'` ]{1,255}$
Validator.DirectoryName=^[a-zA-Z0-9:/\\\\!@#$%^&{}\\[\\]()_+\\-=,.~'` ]{1,255}$
# Validation of dates. Controls whether or not 'lenient' dates are accepted.
# See DataFormat.setLenient(boolean flag) for further details.
Validator.AcceptLenientDates=false
```



validation.properties ,添加到classpath

```
# The ESAPI validator does many security checks on input, such as canonicalization
# and whitelist validation. Note that all of these validation rules are applied *after*
# canonicalization. Double-encoded characters (even with different encodings involved,
# are never allowed.
#
# To use:
#
# First set up a pattern below. You can choose any name you want, prefixed by the word
# "Validation." For example:
#   Validation.Email=^[A-Za-z0-9._%-]+@[A-Za-z0-9.-]+\\.[a-zA-Z]{2,4}$
# 
# Then you can validate in your code against the pattern like this:
#     ESAPI.validator().isValidInput("User Email", input, "Email", maxLength, allowNull);
# Where maxLength and allowNull are set for you needs, respectively.
#
# But note, when you use boolean variants of validation functions, you lose critical 
# canonicalization. It is preferable to use the "get" methods (which throw exceptions) and 
# and use the returned user input which is in canonical form. Consider the following:
#  
# try {
#    someObject.setEmail(ESAPI.validator().getValidInput("User Email", input, "Email", maxLength, allowNull));
#
Validator.SafeString=^[.\\p{Alnum}\\p{Space}]{0,1024}$
Validator.Email=^[A-Za-z0-9._%'-]+@[A-Za-z0-9.-]+\\.[a-zA-Z]{2,4}$
Validator.IPAddress=^(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$
Validator.URL=^(ht|f)tp(s?)\\:\\/\\/[0-9a-zA-Z]([-.\\w]*[0-9a-zA-Z])*(:(0-9)*)*(\\/?)([a-zA-Z0-9\\-\\.\\?\\,\\:\\'\\/\\\\\\+=&;%\\$#_]*)?$
Validator.CreditCard=^(\\d{4}[- ]?){3}\\d{4}$
Validator.SSN=^(?!000)([0-6]\\d{2}|7([0-6]\\d|7[012]))([ -]?)(?!00)\\d\\d\\3(?!0000)\\d{4}$
```



过滤器代码 XXSSProtectionFilter.java
```java

package com.wisdombud.cqupt.edu.web.filter;

import org.apache.commons.lang3.StringUtils;
import org.owasp.esapi.ESAPI;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletOutputStream;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class XXSSProtectionFilter implements Filter {

    private static final Logger LOGGER = LoggerFactory.getLogger(XXSSProtectionFilter.class);

    public static final String CONTENT_SECURITY_POLICY_HEADER = "Content-Security-Policy";
    public static final String CONTENT_SECURITY_POLICY_REPORT_ONLY_HEADER = "Content-Security-Policy-Report-Only";

    /**
     * Instruct the browser to only send reports (does not block anything)
     */
    private static final String REPORT_ONLY = "report-only";
    /**
     * Instructs the browser to POST a reports of policy failures to this URI
     */
    public static final String REPORT_URI = "report-uri";
    /**
     * Enables a sandbox for the requested resource similar to the iframe sandbox attribute.
     * The sandbox applies a same origin policy, prevents popups, plugins and script execution is blocked.
     * You can keep the sandbox value empty to keep all restrictions in place, or add values:
     * allow-forms allow-same-origin allow-scripts, and allow-top-navigation
     */
    public static final String SANDBOX = "sandbox";
    /**
     * The default policy for loading content such as JavaScript, Images, CSS, Font's, AJAX requests, Frames, HTML5
     * Media
     */
    public static final String DEFAULT_SRC = "default-src";
    /**
     * Defines valid sources of images
     */
    public static final String IMG_SRC = "img-src";
    /**
     * Defines valid sources of JavaScript
     */
    public static final String SCRIPT_SRC = "script-src";
    /**
     * Defines valid sources of stylesheets
     */
    public static final String STYLE_SRC = "style-src";
    /**
     * Defines valid sources of fonts
     */
    public static final String FONT_SRC = "font-src";
    /**
     * Applies to XMLHttpRequest (AJAX), WebSocket or EventSource
     */
    public static final String CONNECT_SRC = "connect-src";
    /**
     * Defines valid sources of plugins, eg <object>, <embed> or <applet>.
     */
    public static final String OBJECT_SRC = "object-src";
    /**
     * Defines valid sources of audio and video, eg HTML5 <audio>, <video> elements
     */
    public static final String MEDIA_SRC = "media-src";
    /**
     * Defines valid sources for loading frames
     */
    public static final String FRAME_SRC = "frame-src";

    public static final String KEYWORD_NONE = "'none'";
    public static final String KEYWORD_SELF = "'self'";

    private boolean reportOnly;
    private String reportUri;
    private String sandbox;
    private String defaultSrc;
    private String imgSrc;
    private String scriptSrc;
    private String styleSrc;
    private String fontSrc;
    private String connectSrc;
    private String objectSrc;
    private String mediaSrc;
    private String frameSrc;

    public void init(final FilterConfig filterConfig) {
        this.reportOnly = getParameterBooleanValue(filterConfig, REPORT_ONLY);
        this.reportUri = getParameterValue(filterConfig, REPORT_URI);
        this.sandbox = getParameterValue(filterConfig, SANDBOX);
        this.defaultSrc = getParameterValue(filterConfig, DEFAULT_SRC, KEYWORD_NONE);
        this.imgSrc = getParameterValue(filterConfig, IMG_SRC);
        this.scriptSrc = getParameterValue(filterConfig, SCRIPT_SRC);
        this.styleSrc = getParameterValue(filterConfig, STYLE_SRC);
        this.fontSrc = getParameterValue(filterConfig, FONT_SRC);
        this.connectSrc = getParameterValue(filterConfig, CONNECT_SRC);
        this.objectSrc = getParameterValue(filterConfig, OBJECT_SRC);
        this.mediaSrc = getParameterValue(filterConfig, MEDIA_SRC);
        this.frameSrc = getParameterValue(filterConfig, FRAME_SRC);
    }

    @Override
    public void doFilter(final ServletRequest req, final ServletResponse res, final FilterChain chain)
        throws IOException, ServletException {

        final HttpServletRequest request = (HttpServletRequest) req;
        final HttpServletResponse response = (HttpServletResponse) res;

        if (response == null) {
            LOGGER.error("Unable to retrieve HttpServletResponse from invocation context");
        } else {
            response.addHeader("X-XSS-Protection", "1; mode=block");
            response.addHeader("X-Frame-Option", "SAMEORIGIN");

            final String contentSecurityPolicyHeaderName =
                this.reportOnly ? CONTENT_SECURITY_POLICY_REPORT_ONLY_HEADER : CONTENT_SECURITY_POLICY_HEADER;
            final String contentSecurityPolicy = getContentSecurityPolicy();

            LOGGER.debug("Adding Header {} = {}", contentSecurityPolicyHeaderName, contentSecurityPolicy);
            response.addHeader(contentSecurityPolicyHeaderName, contentSecurityPolicy);

            //对response的返回值进行xss过滤
            final ResponseWrapper wrapperResponse = new ResponseWrapper(response); //转换成代理类
            // 这里只拦截返回，直接让请求过去，如果在请求前有处理，可以在这里处理
            final byte[] content = wrapperResponse.getContent(); //获取返回值
            if (content.length > 0) {

                final String str = new String(content, "UTF-8");
                System.out.println("返回值:" + str);
                String ciphertext = null;

                try {
                    ciphertext = ESAPI.encoder().encodeForHTMLAttribute(str);
                } catch (final Exception e) {
                    e.printStackTrace();
                }
                final ServletOutputStream out = response.getOutputStream();
                out.write(ciphertext.getBytes());
                out.flush();
                out.close();
            }
        }

        chain.doFilter(request, response);

    }

    @Override
    public void destroy() {

    }

    private String getContentSecurityPolicy() {
        final StringBuilder contentSecurityPolicy = new StringBuilder(DEFAULT_SRC).append(" ").append(this.defaultSrc);

        addDirectiveToContentSecurityPolicy(contentSecurityPolicy, IMG_SRC, this.imgSrc);
        addDirectiveToContentSecurityPolicy(contentSecurityPolicy, SCRIPT_SRC, this.scriptSrc);
        addDirectiveToContentSecurityPolicy(contentSecurityPolicy, STYLE_SRC, this.styleSrc);
        addDirectiveToContentSecurityPolicy(contentSecurityPolicy, FONT_SRC, this.fontSrc);
        addDirectiveToContentSecurityPolicy(contentSecurityPolicy, CONNECT_SRC, this.connectSrc);
        addDirectiveToContentSecurityPolicy(contentSecurityPolicy, OBJECT_SRC, this.objectSrc);
        addDirectiveToContentSecurityPolicy(contentSecurityPolicy, MEDIA_SRC, this.mediaSrc);
        addDirectiveToContentSecurityPolicy(contentSecurityPolicy, FRAME_SRC, this.frameSrc);
        addDirectiveToContentSecurityPolicy(contentSecurityPolicy, REPORT_URI, this.reportUri);
        addDirectiveToContentSecurityPolicy(contentSecurityPolicy, "plugin-types", "application/x-shockwave-flash");

        return contentSecurityPolicy.toString();
    }

    private String getParameterValue(final FilterConfig filterConfig, final String paramName,
        final String defaultValue) {
        String value = filterConfig.getInitParameter(paramName);
        if (StringUtils.isBlank(value)) {
            value = defaultValue;
        }
        return value;
    }

    private String getParameterValue(final FilterConfig filterConfig, final String paramName) {
        return filterConfig.getInitParameter(paramName);
    }

    private boolean getParameterBooleanValue(final FilterConfig filterConfig, final String paramName) {
        return "true".equalsIgnoreCase(filterConfig.getInitParameter(paramName));
    }

    private void addDirectiveToContentSecurityPolicy(final StringBuilder contentSecurityPolicy,
        final String directiveName, final String value) {
        if (StringUtils.isNotBlank(value) && !this.defaultSrc.equals(value)) {
            contentSecurityPolicy.append("; ").append(directiveName).append(" ").append(value);
        }
    }

    private void addSandoxDirectiveToContentSecurityPolicy(final StringBuilder contentSecurityPolicy,
        final String value) {
        if (StringUtils.isNotBlank(value)) {
            if ("true".equalsIgnoreCase(value)) {
                contentSecurityPolicy.append("; ").append(SANDBOX);
            } else {
                contentSecurityPolicy.append("; ").append(SANDBOX).append(" ").append(value);
            }
        }
    }

}

```
