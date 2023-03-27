
```bash
java -XX:+PrintCommandLineFlags -version
```

실행 결과
```bash
-XX:ConcGCThreads=3 -XX:G1ConcRefinementThreads=10 -XX:GCDrainStackTargetSize=64 -XX:InitialHeapSize=536870912 -XX:MarkStackSize=4194304 -XX:MaxHeapSize=8589934592 -XX:MinHeapSize=6815736 -XX:+PrintCommandLineFlags -XX:ReservedCodeCacheSize=251658240 -XX:+SegmentedCodeCache -XX:+UseCompressedClassPointers -XX:+UseCompressedOops -XX:+UseG1GC
openjdk version "17" 2021-09-14
OpenJDK Runtime Environment (build 17+35-2724)
OpenJDK 64-Bit Server VM (build 17+35-2724, mixed mode, sharing)
```
- 내가 사용하고있는 openjdk 17 버전의 기본 GC는 G1GC이다 (`-XX:+UseG1GC`)

참고
- https://stackoverflow.com/questions/33206313/default-garbage-collector-for-java-8
