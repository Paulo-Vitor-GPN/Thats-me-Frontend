[05/12/23 12:13:11:522 BRST] 00000094 SystemErr     R Caused by: com.ibm.ws.webcontainer.exception.WebAppNotLoadedException: Failed to load webapp: Failed to load webapp: javax/faces/component/visit/VisitHint.SKIP_ITERATION
[05/12/23 12:13:11:522 BRST] 00000094 SystemErr     R 	at com.ibm.ws.webcontainer.WSWebContainer.addWebApp(WSWebContainer.java:914)
[05/12/23 12:13:11:522 BRST] 00000094 SystemErr     R 	at com.ibm.ws.webcontainer.WSWebContainer.addWebApplication(WSWebContainer.java:789)
[05/12/23 12:13:11:522 BRST] 00000094 SystemErr     R 	at com.ibm.ws.webcontainer.component.WebContainerImpl.install(WebContainerImpl.java:427)
[05/12/23 12:13:11:522 BRST] 00000094 SystemErr     R 	... 97 more
[05/12/23 12:13:11:522 BRST] 00000094 SystemErr     R Caused by: com.ibm.ws.webcontainer.exception.WebAppNotLoadedException: Failed to load webapp: javax/faces/component/visit/VisitHint.SKIP_ITERATION
[05/12/23 12:13:11:522 BRST] 00000094 SystemErr     R 	at com.ibm.ws.webcontainer.VirtualHostImpl.addWebApplication(VirtualHostImpl.java:186)
[05/12/23 12:13:11:522 BRST] 00000094 SystemErr     R 	at com.ibm.ws.webcontainer.WSWebContainer.addWebApp(WSWebContainer.java:904)
[05/12/23 12:13:11:522 BRST] 00000094 SystemErr     R 	... 99 more
[05/12/23 12:13:11:522 BRST] 00000094 SystemErr     R Caused by: java.lang.NoSuchFieldError: javax/faces/component/visit/VisitHint.SKIP_ITERATION
[05/12/23 12:13:11:522 BRST] 00000094 SystemErr     R 	at org.icefaces.impl.event.RestoreResourceDependencies.<clinit>(RestoreResourceDependencies.java:39)
[05/12/23 12:13:11:522 BRST] 00000094 SystemErr     R 	at java.lang.J9VMInternals.newInstanceImpl(Native Method)
[05/12/23 12:13:11:522 BRST] 00000094 SystemErr     R 	at java.lang.Class.newInstance(Class.java:1846)
[05/12/23 12:13:11:522 BRST] 00000094 SystemErr     R 	at org.apache.myfaces.shared_impl.util.ClassUtils.newInstance(ClassUtils.java:362)
[05/12/23 12:13:11:523 BRST] 00000094 SystemErr     R 	at org.apache.myfaces.shared_impl.util.ClassUtils.newInstance(ClassUtils.java:327)
[05/12/23 12:13:11:523 BRST] 00000094 SystemErr     R 	at org.apache.myfaces.config.FacesConfigurator.configureApplication(FacesConfigurator.java:729)
[05/12/23 12:13:11:523 BRST] 00000094 SystemErr     R 	at org.apache.myfaces.config.FacesConfigurator.configure(FacesConfigurator.java:460)
[05/12/23 12:13:11:523 BRST] 00000094 SystemErr     R 	at org.apache.myfaces.webapp.AbstractFacesInitializer.buildConfiguration(AbstractFacesInitializer.java:350)
[05/12/23 12:13:11:523 BRST] 00000094 SystemErr     R 	at org.apache.myfaces.webapp.Jsp21FacesInitializer.initContainerIntegration(Jsp21FacesInitializer.java:73)
[05/12/23 12:13:11:523 BRST] 00000094 SystemErr     R 	at org.apache.myfaces.webapp.AbstractFacesInitializer.initFaces(AbstractFacesInitializer.java:143)
[05/12/23 12:13:11:523 BRST] 00000094 SystemErr     R 	at org.apache.myfaces.webapp.StartupServletContextListener.contextInitialized(StartupServletContextListener.java:111)
[05/12/23 12:13:11:523 BRST] 00000094 SystemErr     R 	at com.ibm.ws.webcontainer.webapp.WebApp.notifyServletContextCreated(WebApp.java:1736)
[05/12/23 12:13:11:523 BRST] 00000094 SystemErr     R 	at com.ibm.ws.webcontainer.webapp.WebAppImpl.initialize(WebAppImpl.java:415)
[05/12/23 12:13:11:523 BRST] 00000094 SystemErr     R 	at com.ibm.ws.webcontainer.webapp.WebGroupImpl.addWebApplication(WebGroupImpl.java:88)
[05/12/23 12:13:11:523 BRST] 00000094 SystemErr     R 	at com.ibm.w
