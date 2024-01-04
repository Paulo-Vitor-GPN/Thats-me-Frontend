[ERROR] 

[2024-01-03T22:47:34.626Z] java.lang.RuntimeException: Could not generate model 'Stimulus'

[2024-01-03T22:47:34.626Z]     at io.swagger.codegen.v3.DefaultGenerator.generateModels (DefaultGenerator.java:451)

[2024-01-03T22:47:34.626Z]     at io.swagger.codegen.v3.DefaultGenerator.generate (DefaultGenerator.java:779)

[2024-01-03T22:47:34.626Z]     at io.swagger.codegen.v3.maven.plugin.CodeGenMojo.execute_ (CodeGenMojo.java:554)

[2024-01-03T22:47:34.626Z]     at io.swagger.codegen.v3.maven.plugin.CodeGenMojo.execute (CodeGenMojo.java:319)

[2024-01-03T22:47:34.626Z]     at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo (DefaultBuildPluginManager.java:134)

[2024-01-03T22:47:34.626Z]     at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:208)

[2024-01-03T22:47:34.626Z]     at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:154)

[2024-01-03T22:47:34.626Z]     at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:146)

[2024-01-03T22:47:34.626Z]     at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:117)

[2024-01-03T22:47:34.626Z]     at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:81)

[2024-01-03T22:47:34.626Z]     at org.apache.maven.lifecycle.internal.builder.singlethreaded.SingleThreadedBuilder.build (SingleThreadedBuilder.java:51)

[2024-01-03T22:47:34.626Z]     at org.apache.maven.lifecycle.internal.LifecycleStarter.execute (LifecycleStarter.java:128)

[2024-01-03T22:47:34.626Z]     at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:309)

[2024-01-03T22:47:34.626Z]     at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:194)

[2024-01-03T22:47:34.626Z]     at org.apache.maven.DefaultMaven.execute (DefaultMaven.java:107)

[2024-01-03T22:47:34.626Z]     at org.apache.maven.cli.MavenCli.execute (MavenCli.java:955)

[2024-01-03T22:47:34.626Z]     at org.apache.maven.cli.MavenCli.doMain (MavenCli.java:290)

[2024-01-03T22:47:34.626Z]     at org.apache.maven.cli.MavenCli.main (MavenCli.java:194)

[2024-01-03T22:47:34.626Z]     at jdk.internal.reflect.NativeMethodAccessorImpl.invoke0 (Native Method)

[2024-01-03T22:47:34.626Z]     at jdk.internal.reflect.NativeMethodAccessorImpl.invoke (NativeMethodAccessorImpl.java:77)

[2024-01-03T22:47:34.626Z]     at jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke (DelegatingMethodAccessorImpl.java:43)

[2024-01-03T22:47:34.626Z]     at java.lang.reflect.Method.invoke (Method.java:568)

[2024-01-03T22:47:34.626Z]     at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced (Launcher.java:289)

[2024-01-03T22:47:34.627Z]     at org.codehaus.plexus.classworlds.launcher.Launcher.launch (Launcher.java:229)

[2024-01-03T22:47:34.627Z]     at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode (Launcher.java:415)

[2024-01-03T22:47:34.627Z]     at org.codehaus.plexus.classworlds.launcher.Launcher.main (Launcher.java:356)

[2024-01-03T22:47:34.627Z] Caused by: com.github.jknack.handlebars.HandlebarsException: /handlebars/JavaSpring/pojo.mustache:2:6: java.lang.reflect.InaccessibleObjectException: Unable to make public boolean java.util.Collections$EmptyMap.isEmpty() accessible: module java.base does not "opens java.util" to unnamed module @3dec79f8
