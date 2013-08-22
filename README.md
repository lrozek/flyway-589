flyway-589
==========

bug reproduction for https://github.com/flyway/flyway/issues/589


env required:
SQL SERVER 2012, but I quess it will also fail on previous versions


steps to reproduce:
1) in flyway-settings.xml provide configuration for database
2) mvn clean compile flyway:migrate -s ../flyway-settings.xml
3) mvn clean compile flyway:clean -s ../flyway-settings.xml