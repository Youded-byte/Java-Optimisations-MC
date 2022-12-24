# Java-Optimisations-MC
This repository serves to provide effective ways of improving performance in minecraft, with a focus on lunar client, vanilla and lightweight modpacks using different Java distributions and options.
It is the goal of this repository to have discourse over different java configurations and the effects they have, and provide simple access to different configurations.
Please open an issue if you have a possible configuration with improved performance, with benchmarks to support them.

## Java Distribution
The GraalVM distribution of Java, is made by Oracle, and is equipped with an [advanced](https://www.oracle.com/java/graalvm/) Just-In-Time (JIT) compiler that possibly has [the best performance](https://renaissance.dev/) for long-during intensive tasks. The newest version of the [the community version](https://github.com/graalvm/graalvm-ce-builds/releases/tag/vm-22.0.0.2) is open-source. The [enterprise edition](https://www.oracle.com/downloads/graalvm-downloads.html#license-lightbox) is closed-source but may have slight improvements over the community edition, but requires registration at Oracle's website. Lunar Client requires a Java version of 16 or above. The latest Minecraft versions also require Java 17. GraalVM provides Java 17, which is recommended.

## Java Options
For the Enterprise Edition:
```yml
-Xverify:none -Xss2M -Xmn1G -XX:+UnlockExperimentalVMOptions -XX:+UseG1GC -XX:MaxGCPauseMillis=40 -XX:+AlwaysActAsServerClassMachine -XX:MaxTenuringThreshold=1 -XX:SurvivorRatio=32 -XX:G1HeapRegionSize=8M -XX:G1MixedGCCountTarget=4 -XX:G1MixedGCLiveThresholdPercent=90 -XX:-UsePerfData -XX:+PerfDisableSharedMem -XX:+UseLargePages -XX:+AlwaysPreTouch -XX:+UseFastStosb -XX:+EliminateLocks -Dgraal.TuneInlinerExploration=1 -XX:+EagerJVMCI

```
For the Community Edition / other JRE:
```yml
-Xverify:none -Xss2M -Xmn1G -XX:+UnlockExperimentalVMOptions -XX:+UseG1GC -XX:MaxGCPauseMillis=40 -XX:+AlwaysActAsServerClassMachine -XX:MaxTenuringThreshold=1 -XX:SurvivorRatio=32 -XX:G1HeapRegionSize=8M -XX:G1MixedGCCountTarget=4 -XX:G1MixedGCLiveThresholdPercent=90 -XX:-UsePerfData -XX:+PerfDisableSharedMem -XX:+UseLargePages -XX:+AlwaysPreTouch -XX:+UseFastStosb -XX:+EliminateLocks -XX:+EnableJVMCIProduct -XX:+EnableJVMCI -XX:+UseJVMCICompiler -XX:+EagerJVMCI

```
* Xverify:none causes bytecode not to be checked during runtime. This option is safe to disable on trusted code and speeds up load times.
* Xss2M sets the stacks size from the default 1M on 64-bit Java, to 2M. I have noticed a slight increase in performance with this option.
* Xmn1G sets the size of the heap for young generation objects i.e. those with short life-span to 1 gigabyte. This option's effects have been [documented](https://hypixel.net/threads/getting-better-fps-stablity-by-using-another-jre-with-lunar-client.4518890/).
* XX:+UseG1GC uses the G1C compiler, enabled on default on most Java distributions and fast.
* XX:MaxGCPauseMillis=40 sets time allowed for garbage collection. A shorter number decreases latency, desirable for a game.
* XX:MaxTenuringThreshold=1 sets the Maximum Tenuring Treshold to 1, resulting in more objects being considered longer lasting and kept away from the GC.
* XX:SurvivorRatio=32 causes more memory to "survive" from the GC, saving processing power.
* XX:G1HeapRegionSize=8M sets the G1 garbage collectors heap region size to 8M, by increasing this value, memory can be more easily freed.
* XX:G1MixedGCCountTarget=4 sets expected length for space-reclamation to 4 from 8, causing garbage collection to be done slower.
* XX:G1MixedGCLiveThresholdPercent=90 increases the percentage of memory that does not have the be processed by the GC if it is longer lasting.
* server enables experimental options in the JVM, among with using the optimal garbage collector.
* XX:-UsePerfData disables data collection and writing of information about performance into files, which may slightly increase performance.
* XX:+PerfDisableSharedMem disables some data collection and features of the JVM, but they are not needed in normal usage of Minecraft.
* XX:+UseLargePages enables usage of large pages which may improve performance. You **must give the user access to this right which is not given by default**. To use this feature refer to [Oracle's website](https://www.oracle.com/java/technologies/javase/largememory-pages.html). On linux you will want to replace it with XX:+UseTransparentHugePages.
* XX:+AlwaysPreTouch results in performance improvements, with the reason described [here](https://access.redhat.com/solutions/2685771).
* XX:+UseFastStosb uses fast-string operations.
* XX:+EliminateLocks enhances performance when multiple threads.
* Dgraal.TuneInlinerExploration=1 is an option that in my testing has improved performance on 1. Information is about it is [here](https://www.graalvm.org/22.0/reference-manual/java/options/). It only takes effect on the enterprise edition of GraalVM.
* XX:+EagerJVMCI enables the Just-In-Time (JIT) compiler in GraalVM.

## Enabling Large Pages on Windows
While Large Pages works on default on Linux, on Windows you need to go to Local Security Policy, there into Local Policies, then into User Rights Assignment where you can double click on Lock pages in memory. You can add the Group Users, or only your own user as you want.

## Discourse resulting in improvement
Different java configurations work differently on different systems, but a 64-bit configuration with about 2-4 GB of RAM available to be allocated to minecraft and at least 4 cores is targeted. If you have suggestions for improvements, please explain why your change would improve performance, and it is best if you provide a simple benchmark, using for example [CapFrameX](https://github.com/CXWorld/CapFrameX).
