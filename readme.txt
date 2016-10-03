

1) Create the remote interface in code(EJBLocal in jboss 4).

package business.ejb.login;

import javax.ejb.Remote;
@Remote
public interface login {

	public boolean verifyLogin(String user, String password);          /// method to implement

2) change the extend class from SessionBean to login(interface).

3)Remove the follwoing method from bean class.

public void ejbCreate() throws CreateException {}
   
	public void setSessionContext( SessionContext sessionContext ) {}
	
   public void ejbActivate() {}
	
   public void ejbPassivate() {}
	
   public void ejbRemove() {}

4) Remove the ejb-jar.xml in META-INF folder.
5) call the ejb lookup like following
 from
//LoginVerifierHome lHome = (LoginVerifierHome) lContext.lookup("LoginVerifierBean"); 

to
login bean=(login) ctx.lookup("java:global/libra-ejb/LoginVerifierBean!business.ejb.login.login");

6) call the interface directly in method.

from

LoginVerifierLocal h1 = lHome.create();
(if (bean.verifyLogin(name,password))


to 

if (bean.verifyLogin(name,password)) 


7) Added the jsp-config tag for tld in web.xml 

<jsp-config>

...........................
<taglib>
  <taglib-uri>/WEB-INF/tlds/struts-template.tld</taglib-uri>
  <taglib-location>/WEB-INF/tlds/struts-tlds/struts-template.tld</taglib-location>
  </taglib>
.........
</jsp-config>


8) make Book.java class to serailizable class.

public class Book implements java.io.Serializable {


9) change the lookup from java:/libra to java:/jboss/libra in EdiaryDBConnection.java

10) change the url in LibraDBConnection.java

private String url = "jdbc:mysql://192.168.1.80/libra?user=root&password=redhat";


11) Added the follwoing properties for message driven bean(BookOrderReceiverBean)

@MessageDriven(name = "MessageMDBSample", activationConfig = {
		 @ActivationConfigProperty(propertyName = "destinationType", propertyValue = "javax.jms.Queue"),
		 @ActivationConfigProperty(propertyName = "destination", propertyValue = "queue/testQueue"),
		 @ActivationConfigProperty(propertyName = "acknowledgeMode", propertyValue = "Auto-acknowledge") })

12) created queue in jboss eap 6


public class BookOrderReceiverBean implements MessageDrivenBean, MessageListener {

