flyway-589
==========

bug reproduction for https://github.com/flyway/flyway/issues/589


env required:
SQL SERVER 2012, but I quess it will also fail on previous versions


steps to reproduce:
1) in flyway-settings.xml provide configuration for database
2) mvn clean compile flyway:migrate -s ../flyway-settings.xml
3) mvn clean compile flyway:clean -s ../flyway-settings.xml -X
4) following exception should be reported

[ERROR] Failed to execute goal com.googlecode.flyway:flyway-maven-plugin:2.2.1:clean (default-cli) on project migration.executor: com.googlecode.flyway.core.api.FlywayException: Unable to clean schema [dbo]: Cannot DROP FUNCTION 'dbo.CHECK_AGE' because it is being referenced by object 'NO_OLDER_THAN_100_CONSTRAINT'. -> [Help 1]
org.apache.maven.lifecycle.LifecycleExecutionException: Failed to execute goal com.googlecode.flyway:flyway-maven-plugin:2.2.1:clean (default-cli) on project migration.executor: com.googlecode.flyway.core.api.FlywayException: Unable to clean schema [dbo]
	at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:217)
	at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:153)
	at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:145)
	at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:84)
	at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:59)
	at org.apache.maven.lifecycle.internal.LifecycleStarter.singleThreadedBuild(LifecycleStarter.java:183)
	at org.apache.maven.lifecycle.internal.LifecycleStarter.execute(LifecycleStarter.java:161)
	at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:320)
	at org.apache.maven.DefaultMaven.execute(DefaultMaven.java:156)
	at org.apache.maven.cli.MavenCli.execute(MavenCli.java:537)
	at org.apache.maven.cli.MavenCli.doMain(MavenCli.java:196)
	at org.apache.maven.cli.MavenCli.main(MavenCli.java:141)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:606)
	at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced(Launcher.java:290)
	at org.codehaus.plexus.classworlds.launcher.Launcher.launch(Launcher.java:230)
	at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode(Launcher.java:409)
	at org.codehaus.plexus.classworlds.launcher.Launcher.main(Launcher.java:352)
Caused by: org.apache.maven.plugin.MojoExecutionException: com.googlecode.flyway.core.api.FlywayException: Unable to clean schema [dbo]
	at com.googlecode.flyway.maven.AbstractFlywayMojo.execute(AbstractFlywayMojo.java:253)
	at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo(DefaultBuildPluginManager.java:101)
	at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:209)
	... 19 more
Caused by: java.sql.SQLException: Cannot DROP FUNCTION 'dbo.CHECK_AGE' because it is being referenced by object 'NO_OLDER_THAN_100_CONSTRAINT'.
	at net.sourceforge.jtds.jdbc.SQLDiagnostic.addDiagnostic(SQLDiagnostic.java:372)
	at net.sourceforge.jtds.jdbc.TdsCore.tdsErrorToken(TdsCore.java:2988)
	at net.sourceforge.jtds.jdbc.TdsCore.nextToken(TdsCore.java:2421)
	at net.sourceforge.jtds.jdbc.TdsCore.getMoreResults(TdsCore.java:671)
	at net.sourceforge.jtds.jdbc.JtdsStatement.processResults(JtdsStatement.java:613)
	at net.sourceforge.jtds.jdbc.JtdsStatement.executeSQL(JtdsStatement.java:572)
	at net.sourceforge.jtds.jdbc.JtdsPreparedStatement.execute(JtdsPreparedStatement.java:784)
	at com.googlecode.flyway.core.dbsupport.JdbcTemplate.execute(JdbcTemplate.java:214)
	at com.googlecode.flyway.core.dbsupport.sqlserver.SQLServerSchema.doClean(SQLServerSchema.java:85)
	at com.googlecode.flyway.core.dbsupport.Schema.clean(Schema.java:148)
	at com.googlecode.flyway.core.command.DbClean$2.doInTransaction(DbClean.java:119)
	at com.googlecode.flyway.core.command.DbClean$2.doInTransaction(DbClean.java:117)
	at com.googlecode.flyway.core.util.jdbc.TransactionTemplate.execute(TransactionTemplate.java:56)
	at com.googlecode.flyway.core.command.DbClean.cleanSchema(DbClean.java:117)
	at com.googlecode.flyway.core.command.DbClean.clean(DbClean.java:81)
	at com.googlecode.flyway.core.Flyway$3.execute(Flyway.java:933)
	at com.googlecode.flyway.core.Flyway$3.execute(Flyway.java:929)
	at com.googlecode.flyway.core.Flyway.execute(Flyway.java:1200)
	at com.googlecode.flyway.core.Flyway.clean(Flyway.java:929)
	at com.googlecode.flyway.maven.CleanMojo.doExecute(CleanMojo.java:32)
	at com.googlecode.flyway.maven.AbstractFlywayMojo.execute(AbstractFlywayMojo.java:251)
	... 21 more

