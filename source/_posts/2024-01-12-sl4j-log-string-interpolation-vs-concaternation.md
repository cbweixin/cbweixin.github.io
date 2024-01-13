title: sl4j-log-string-interpolation-vs-concaternation
date: 2024-01-12 18:04:20
categories: programming
tags: java
---
I am curious about the performance of log string concatenation vs interpolation, today I did some test.

# code
test code:

```java

package com.ap.core.pipeline.shared.io.logger;

import java.util.concurrent.TimeUnit;
import lombok.extern.slf4j.Slf4j;
import org.openjdk.jmh.annotations.*;
import org.openjdk.jmh.runner.Runner;
import org.openjdk.jmh.runner.RunnerException;
import org.openjdk.jmh.runner.options.Options;
import org.openjdk.jmh.runner.options.OptionsBuilder;

@BenchmarkMode(Mode.AverageTime)
@OutputTimeUnit(TimeUnit.MICROSECONDS)
@State(Scope.Benchmark)
@Slf4j
public class LoggerTest {

    @Param({"100", "1000", "10000", "100000"})
    public int setSize;

    @Benchmark
	    public void logoutputWithConcatnationShort() {
		    for (int i = 0; i < setSize; i++) {
            log.info("abc askdfj aldfj " + "afdakl");
        
		    }
    
	    }


    @Benchmark
	    public void logoutputWithConcatnationMedium() {
		    for (int i = 0; i < setSize; i++) {
            log.info("abc askdfj aldfj " + "afdakl " + " asdfa " + " kadjfka d " + " alsdjflas");
        
		    }
    
	    }

    @Benchmark
	    public void logoutputWithConcatnationLong() {
		    for (int i = 0; i < setSize; i++) {
			    log.info(
                "abc askdfj aldfj " + "afdakl " + " asdfa " + " kadjfka d " + " alsdjflas " + " asfas;a " + " akdjfakj "
                    + " ladjla kdfaks dfla " + " laksdjfklas ljaf;", " asdfjla fldsj;"
				    );
        
		    }
    
	    }

    @Benchmark
	    public void logoutputWithInterpolationShort() {
		    for (int i = 0; i < setSize; i++) {
            log.info("{} {}", "abc askdfj aldfj ", "afdakl");
        
		    }
    
	    }


    @Benchmark
	    public void logoutputWithInterpolationMedium() {
		    for (int i = 0; i < setSize; i++) {
            log.info("{} {} {} {} {}", "abc askdfj aldfj ", "afdakl ", " asdfa ", " kadjfka d ", " alsdjflas");
        
		    }
    
	    }

    @Benchmark
	    public void logoutputWithInterpolationLong() {
		    for (int i = 0; i < setSize; i++) {
            log.info( "{} {} {} {} {} {} {} {} {} {}"
                ,"abc askdfj aldfj " , "afdakl " , " asdfa " , " kadjfka d " , " alsdjflas " , " asfas;a " , " akdjfakj "
                    , " ladjla kdfaks dfla " , " laksdjfklas ljaf; ", " asdfjla fldsj;" );
        
		    }
    
	    }


    public static void main(String... args) throws RunnerException {
        Options opts = new OptionsBuilder()
            .include(".*")
            .warmupIterations(1)
            .measurementIterations(2)
            .jvmArgs("-Xms2g", "-Xmx2g")
            .shouldDoGC(true)
            .forks(1)
            .build();

        new Runner(opts).run();
    
    }

}

```

# result
```
Benchmark                                    (setSize)  Mode  Cnt        Score   Error  Units
LoggerTest.logoutputWithConcatnationLong           100  avgt    2     1171.665          us/op
LoggerTest.logoutputWithInterpolationLong          100  avgt    2     1129.845          us/op

LoggerTest.logoutputWithConcatnationLong          1000  avgt    2    11030.243          us/op
LoggerTest.logoutputWithInterpolationLong         1000  avgt    2    11240.832          us/op

LoggerTest.logoutputWithConcatnationLong         10000  avgt    2   111822.964          us/op
LoggerTest.logoutputWithInterpolationLong        10000  avgt    2   112777.469          us/op

LoggerTest.logoutputWithConcatnationLong        100000  avgt    2  1111121.390          us/op
LoggerTest.logoutputWithInterpolationLong       100000  avgt    2  1157828.241          us/op

LoggerTest.logoutputWithConcatnationMedium         100  avgt    2     1116.976          us/op
LoggerTest.logoutputWithInterpolationMedium        100  avgt    2     1129.969          us/op

LoggerTest.logoutputWithConcatnationMedium        1000  avgt    2    10875.121          us/op
LoggerTest.logoutputWithInterpolationMedium       1000  avgt    2    11237.823          us/op

LoggerTest.logoutputWithConcatnationMedium       10000  avgt    2   109257.577          us/op
LoggerTest.logoutputWithInterpolationMedium      10000  avgt    2   112633.264          us/op

LoggerTest.logoutputWithConcatnationMedium      100000  avgt    2  1096854.506          us/op
LoggerTest.logoutputWithInterpolationMedium     100000  avgt    2  1151104.319          us/op

LoggerTest.logoutputWithConcatnationShort          100  avgt    2     1104.930          us/op
LoggerTest.logoutputWithInterpolationShort         100  avgt    2     1112.007          us/op

LoggerTest.logoutputWithConcatnationShort         1000  avgt    2    11055.935          us/op
LoggerTest.logoutputWithInterpolationShort        1000  avgt    2    11026.309          us/op

LoggerTest.logoutputWithConcatnationShort        10000  avgt    2   111964.877          us/op
LoggerTest.logoutputWithInterpolationShort       10000  avgt    2   111316.884          us/op

LoggerTest.logoutputWithConcatnationShort       100000  avgt    2  1151162.921          us/op
LoggerTest.logoutputWithInterpolationShort      100000  avgt    2  1131141.245          us/op`

```
the first impression is that for slf4j, the string interpolation is almost the same as concatenation. I didn't see a obvious trend which interpolate is faster than concatenation. when the interpolation number is 5 and 10, interpolation actually is slower than concatenation. only when the interpolation number is 1, I saw interpolate is faster. so I think choose interpolation or concatenation doesn't make much differences. 

