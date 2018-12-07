maven archetype


### sms.web


```
mvn archetype:generate -DarchetypeArtifactId=web.archetype -DarchetypeGroupId=com.wisdombud -DarchetypeVersion=0.0.6-SNAPSHOT -DgroupId=com.wisdombud.cqupt -DartifactId=com.wisdombud.cqupt.web.super -Dversion=0.0.1-SNAPSHOT
```

### itim.web
```
mvn archetype:generate -DarchetypeArtifactId=itim.archetype -DarchetypeGroupId=com.wisdombud -DarchetypeVersion=0.0.6-SNAPSHOT -DgroupId=com.wisdombud.cqupt -DartifactId=com.wisdombud.cqupt.web.super -Dversion=0.0.1-SNAPSHOT
```

### daemon

```
mvn archetype:generate -DarchetypeArtifactId=daemon.archetype -DarchetypeGroupId=com.wisdombud -DarchetypeVersion=0.0.1-SNAPSHOT -DgroupId=com.wisdombud.cqupt -DartifactId=com.wisdombud.cqupt.daemon.super -Dversion=0.0.1-SNAPSHOT
```



创建骨架可用
mvn archetype:generate  -DarchetypeCatalog=remote





mvn archetype:generate  -DarchetypeCatalog=local

mvn archetype:generate  -DarchetypeCatalog=internal