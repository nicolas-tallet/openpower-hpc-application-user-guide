---
layout: page
title: JUBE (Juelich Benchmarking Environment)
---
# JUBE (Juelich Benchmarking Environment)

## Reference

* [JUBE 2 Documentation](https://apps.fz-juelich.de/jsc/jube/jube2/docu/index.html)
* [JUBE Tutorial](https://apps.fz-juelich.de/jsc/jube/jube2/docu/tutorial.html)

## Example: Benchmark HPL

* Benchmark File `jube-benchmark-hpl.xml`:
```
<?xml version="1.0" encoding="UTF-8"?>
<jube>
        <parameterset name="parameterset.global">
                <parameter name="execbin" tag="cpu">${execprefix}/bin/xhpl</parameter>
                <parameter name="execbin" tag="gpu">${execprefix}/bin/xhpl_spectrum_xlc_cuda8_new</parameter>
                <parameter name="execprefix">${prefix}/arch/${arch_2}</parameter>
                <parameter name="prefix">~/benchmark/hpl/2.2</parameter>
                <parameter name="srcpkg">${prefix}/src/hpl-2.2.tar.gz</parameter>
        </parameterset>

        <selection>
                <only>HPL</only>
        </selection>

        <benchmark name="HPL" outpath="~/benchmark/hpl/2.2/var">
                <!-- Global -->
                <parameterset name="parameterset.benchmark">
                        <!-- Execution Configuration -->
                        <parameter mode="text" name="smt" type="int">1,2</parameter>
                        <parameter mode="python" name="tasks" type="int">${nodes}*${taskspernode}</parameter>
                        <parameter mode="python" name="threads" type="int">${tasks}*${threadspertask}</parameter>
                        <!-- IBM Spectrum LSF -->
                        <parameter name="esub" separator=";">p8aff(${threadspertask},${smt},1,balance)</parameter>
                        <parameter name="runlimit">01:00</parameter> <!-- HH:MM -->
                </parameterset>
                <!-- CPU -->
                <parameterset name="parameterset.benchmark.cpu" tag="cpu">
                        <!-- HPL.dat File -->
                        <parameter mode="text" name="file_hpl_dat" type="string">HPL.cpu.dat.in</parameter>
                        <!--
                        IBM Power System S822LC "Minsky" | 1x20, 25GB
                        <parameter name="n">57600</parameter><parameter name="nb">160</parameter><parameter name="p">4</parameter><parameter name="q">5</parameter>
                        IBM Power System S822LC "Minsky" | 1x20, 103GB
                        <parameter name="n">117600</parameter><parameter name="nb">168</parameter><parameter name="p">4</parameter><parameter name="q">5</parameter>
                        IBM Power System S822LC "Minsky" | 12x20, 100GB | Full Cluster
                        <parameter name="n">403200</parameter><parameter name="nb">160</parameter><parameter name="p">12</parameter><parameter name="q">20</parameter>
                        -->
                        <parameter name="n">57600</parameter><parameter name="nb">160</parameter><parameter name="p">4</parameter><parameter name="q">5</parameter>
                        <!-- Execution Configuration -->
                        <parameter mode="text" name="nodes" type="int">1</parameter>
                        <parameter mode="text" name="taskspernode" type="int">20</parameter>
                        <parameter mode="text" name="threadspertask" type="int">1</parameter>
                        <!-- IBM Spectrum LSF -->
                        <parameter name="resreq" separator=";">select[hname==${hname}] span[ptile=${taskspernode}]</parameter>
                        <!-- Execution Script -->
                        <parameter name="preprocess" separator=";">/pwrlocal/ibmccmpl/bin/job_prolog.sh -nmon-enable -verbose</parameter>
                        <parameter name="execute" separator=";">time -p ${mpirun} /pwrlocal/ibmccmpl/bin/task_prolog.sh -perf-enable ${execbin}</parameter>
                        <parameter name="postprocess" separator=";">/pwrlocal/ibmccmpl/bin/job_epilog.sh -verbose</parameter>
                </parameterset>
                <!-- GPU -->
                <parameterset name="parameterset.benchmark.gpu" tag="gpu">
                        <!-- HPL.dat File -->
                        <parameter mode="text" name="file_hpl_dat" type="string">HPL.gpu.dat.in</parameter>
                        <!--
                        IBM Power System S822LC "Minsky" | 1x1+1, 15GB
                        <parameter name="n">44160</parameter><parameter name="nb">384</parameter><parameter name="p">1</parameter><parameter name="q">1</parameter>
                        IBM Power System S822LC "Minsky" | 1x2+2, 29GB
                        <parameter name="n">62976</parameter><parameter name="nb">384</parameter><parameter name="p">1</parameter><parameter name="q">2</parameter>
                        IBM Power System S822LC "Minsky" | 1x3+3, GB
                        <parameter name="n">73728</parameter><parameter name="nb">384</parameter><parameter name="p">1</parameter><parameter name="q">3</parameter>
                        IBM Power System S822LC "Minsky" | 1x4+4, 70GB
                        <parameter name="n">96000</parameter><parameter name="nb">384</parameter><parameter name="p">2</parameter><parameter name="q">2</parameter>
                        IBM Power System S822LC "Minsky" | 12x4+4, 70GB | Full Cluster
                        <parameter name="n">331776</parameter><parameter name="nb">384</parameter><parameter name="p">6</parameter><parameter name="q">8</parameter>
                        -->
                        <parameter name="n">96000</parameter><parameter name="nb">384</parameter><parameter name="p">2</parameter><parameter name="q">2</parameter>
                        <!-- Execution Configuration -->
                        <parameter mode="text" name="gpus" type="int">${taskspernode}</parameter>
                        <parameter mode="text" name="nodes" type="int">1</parameter>
                        <parameter mode="python" name="taskspergpu" type="int">${taskspernode}//${gpus}</parameter>
                        <parameter mode="text" name="taskspernode" type="int">4</parameter>
                        <parameter mode="text" name="threadspertask" type="int">5</parameter>
                        <!-- IBM Spectrum LSF -->
                        <parameter name="resreq" separator=";">rusage[ngpus_excl_p=${gpus}] select[hname==${hname}] span[ptile=${taskspernode}]</parameter>
                        <!-- NVIDIA GPU Monitoring -->
                        <parameter export="true" name="GPU_CLOCK_WARNING" tag="gpu">1468</parameter>
                        <parameter export="true" name="GPU_PCIE_GEN_WARNING" tag="gpu">3</parameter>
                        <parameter export="true" name="GPU_PCIE_WIDTH_WARNING" tag="gpu">4</parameter>
                        <parameter export="true" name="GPU_POWER_WARNING" tag="gpu">275</parameter>
                        <parameter export="true" name="GPU_TEMP_WARNING" tag="gpu">80</parameter>
                        <parameter export="true" name="MONITOR_GPU" tag="gpu">1</parameter>
                        <!-- CUDA -->
                        <parameter export="true" name="CUDA_COPY_SPLIT_THRESHOLD_MB" tag="gpu">1</parameter>
                        <parameter export="true" name="CUDA_DEVICE_MAX_CONNECTIONS" tag="gpu">12</parameter>
                        <parameter export="true" name="GPU_DGEMM_SPLIT" tag="gpu">1.00</parameter>
                        <parameter export="true" name="TRSM_CUTOFF" tag="gpu">1000000</parameter>
                        <!-- Execution Script -->
                        <parameter name="preprocess" separator=";">/pwrlocal/ibmccmpl/bin/job_prolog.sh -nmon-enable -querygpu-enable -verbose</parameter>
                        <parameter name="execute" separator=";">time -p ${mpirun} /pwrlocal/ibmccmpl/bin/task_prolog.sh -devices auto -nvprof-enable ${execbin}</parameter>
                        <parameter name="postprocess" separator=";">/pwrlocal/ibmccmpl/bin/job_epilog.sh -verbose</parameter>
                </parameterset>

                <substituteset name="substituteset.benchmark.hpldat">
                        <iofile in="${jube_benchmark_home}/${file_hpl_dat}" out="HPL.dat" />
                        <sub source="#N#" dest="${n}" />
                        <sub source="#NB#" dest="${nb}" />
                        <sub source="#P#" dest="${p}" />
                        <sub source="#Q#" dest="${q}" />
                </substituteset>

                <step iterations="2" name="run">
                        <use from="platform.xml">parameterset.platform</use>
                        <use>parameterset.global</use>
                        <use>parameterset.benchmark</use>
                        <use tag="cpu">parameterset.benchmark.cpu</use>
                        <use tag="gpu">parameterset.benchmark.gpu</use>
                        <use from="platform.xml">substituteset.platform.jobfile</use>
                        <use>substituteset.benchmark.hpldat</use>
                        <do>${bsub} -a "${esub}" -q "restricted" &lt; job.sh</do>
                </step>

                <patternset name="patternset.benchmark">
                        <!-- IBM Spectrum LSF -->
                        <pattern name="host" type="string" unit="">Job was executed on host\(s\) &lt;${jube_pat_wrd}&gt;</pattern>
                        <!--
                        <pattern name="host" type="string" unit="">Job was executed on host\(s\) &lt;${taskspernode}\*${jube_pat_wrd}&gt;</pattern>
                        -->
                        <!-- Performance -->
                        <pattern name="time" type="float" unit="s">^${jube_pat_nwrd}\s*${jube_pat_nint}\s*${jube_pat_nint}\s*${jube_pat_nint}\s*${jube_pat_nint}\s*${jube_pat_fp}\s*${jube_pat_nfp}\s*$</pattern>
                        <pattern name="flops" type="float" unit="GFlops">^${jube_pat_nwrd}\s*${jube_pat_nint}\s*${jube_pat_nint}\s*${jube_pat_nint}\s*${jube_pat_nint}\s*${jube_pat_nfp}\s*${jube_pat_fp}\s*$</pattern>
                        <!-- System Parameters Test -->
                        <!--
                        <pattern name="cpu_bw" type="float" unit="GB/s">^\s*CPU_BW\s*\[GB/s \]\s*${jube_pat_nfp}\s*${jube_pat_nfp}\s*${jube_pat_fp}\s*$</pattern>
                        <pattern name="cpu_fp" type="float" unit="GFlops">^\s*CPU_FP\s*\[GFLPS\]\s*${jube_pat_nfp}\s*${jube_pat_nfp}\s*${jube_pat_fp}\s*$</pattern>
                        <pattern name="h2d_bw" type="float" unit="GB/s">^\s*H2D_BW\s*\[GB/s \]\s*${jube_pat_nfp}\s*${jube_pat_nfp}\s*${jube_pat_fp}\s*$</pattern>
                        <pattern name="d2h_bw" type="float" unit="GB/s">^\s*D2H_BW\s*\[GB/s \]\s*${jube_pat_nfp}\s*${jube_pat_nfp}\s*${jube_pat_fp}\s*$</pattern>
                        <pattern name="bid_bw" type="float" unit="GB/s">^\s*BID_BW\s*\[GB/s \]\s*${jube_pat_nfp}\s*${jube_pat_nfp}\s*${jube_pat_fp}\s*$</pattern>
                        <pattern name="gpu_bw" type="int" unit="GB/s">^\s*GPU_BW\s*\[GB/s \]\s*${jube_pat_nint}\s*${jube_pat_nint}\s*${jube_pat_int}\s*$</pattern>
                        <pattern name="gpu_fp" type="int" unit="GFlops">^\s*NB = 1024\s*${jube_pat_nint}\s*${jube_pat_nint}\s*${jube_pat_int}\s*$</pattern>
                        <pattern name="net_bw" type="int" unit="GB/s">^\s*16384 KB\s*${jube_pat_nint}\s*${jube_pat_nint}\s*${jube_pat_int}\s*$</pattern>
                        <pattern name="net_lat" type="float" unit="us">^\s*NET_LAT\*\[ us  \]\s*${jube_pat_nfp}\s*${jube_pat_nfp}\s*${jube_pat_fp}\s*$</pattern>
                        -->
                        <!-- GPU Monitoring Warnings -->
                        <pattern name="warn_clock" type="int" unit="MHz">^!!!! WARNING:.*Clock: ${jube_pat_int} MHz.*</pattern>
                        <pattern name="warn_power" type="int" unit="W">^!!!! WARNING:.*Power: ${jube_pat_int} W.*</pattern>
                        <pattern name="warn_temp" type="int" unit="C">^!!!! WARNING:.*Temp: ${jube_pat_int} C.*</pattern>
                        <!-- Progress -->
                        <pattern name="flops15" type="float" unit="GFlops">^.*Prog= 1${jube_pat_nint}\.${jube_pat_nint}%\s*.*\s*GF=\s*${jube_pat_fp}\s*.*$</pattern>
                        <pattern name="flops25" type="float" unit="GFlops">^.*Prog= 2${jube_pat_nint}\.${jube_pat_nint}%\s*.*\s*GF=\s*${jube_pat_fp}\s*.*$</pattern>
                        <pattern name="flops35" type="float" unit="GFlops">^.*Prog= 3${jube_pat_nint}\.${jube_pat_nint}%\s*.*\s*GF=\s*${jube_pat_fp}\s*.*$</pattern>
                        <pattern name="flops45" type="float" unit="GFlops">^.*Prog= 4${jube_pat_nint}\.${jube_pat_nint}%\s*.*\s*GF=\s*${jube_pat_fp}\s*.*$</pattern>
                        <pattern name="flops55" type="float" unit="GFlops">^.*Prog= 5${jube_pat_nint}\.${jube_pat_nint}%\s*.*\s*GF=\s*${jube_pat_fp}\s*.*$</pattern>
                        <pattern name="flops65" type="float" unit="GFlops">^.*Prog= 6${jube_pat_nint}\.${jube_pat_nint}%\s*.*\s*GF=\s*${jube_pat_fp}\s*.*$</pattern>
                        <pattern name="flops75" type="float" unit="GFlops">^.*Prog= 7${jube_pat_nint}\.${jube_pat_nint}%\s*.*\s*GF=\s*${jube_pat_fp}\s*.*$</pattern>
                        <pattern name="flops85" type="float" unit="GFlops">^.*Prog= 8${jube_pat_nint}\.${jube_pat_nint}%\s*.*\s*GF=\s*${jube_pat_fp}\s*.*$</pattern>
                        <pattern name="flops95" type="float" unit="GFlops">^.*Prog= 9${jube_pat_nint}\.${jube_pat_nint}%\s*.*\s*GF=\s*${jube_pat_fp}\s*.*$</pattern>
                        <!--
                        <pattern name="nleft15" type="float" unit="#">^.*Prog= 1${jube_pat_nint}\.${jube_pat_nint}%\s*.*N_left= ${jube_pat_int}\s*.*$</pattern>
                        <pattern name="nleft25" type="float" unit="#">^.*Prog= 2${jube_pat_nint}\.${jube_pat_nint}%\s*.*N_left= ${jube_pat_int}\s*.*$</pattern>
                        <pattern name="nleft35" type="float" unit="#">^.*Prog= 3${jube_pat_nint}\.${jube_pat_nint}%\s*.*N_left= ${jube_pat_int}\s*.*$</pattern>
                        <pattern name="nleft45" type="float" unit="#">^.*Prog= 4${jube_pat_nint}\.${jube_pat_nint}%\s*.*N_left= ${jube_pat_int}\s*.*$</pattern>
                        <pattern name="nleft55" type="float" unit="#">^.*Prog= 5${jube_pat_nint}\.${jube_pat_nint}%\s*.*N_left= ${jube_pat_int}\s*.*$</pattern>
                        <pattern name="nleft65" type="float" unit="#">^.*Prog= 6${jube_pat_nint}\.${jube_pat_nint}%\s*.*N_left= ${jube_pat_int}\s*.*$</pattern>
                        <pattern name="nleft75" type="float" unit="#">^.*Prog= 7${jube_pat_nint}\.${jube_pat_nint}%\s*.*N_left= ${jube_pat_int}\s*.*$</pattern>
                        <pattern name="nleft85" type="float" unit="#">^.*Prog= 8${jube_pat_nint}\.${jube_pat_nint}%\s*.*N_left= ${jube_pat_int}\s*.*$</pattern>
                        <pattern name="nleft95" type="float" unit="#">^.*Prog= 9${jube_pat_nint}\.${jube_pat_nint}%\s*.*N_left= ${jube_pat_int}\s*.*$</pattern>
                        -->
                </patternset>

                <analyser name="analyser.benchmark" reduce="false">
                        <use>patternset.benchmark</use>
                        <analyse step="run">
                                <file>HPL.out</file>
                                <file>stdout</file>
                        </analyse>
                </analyser>

                <result>
                        <use>analyser.benchmark</use>
                        <table name="table.benchmark" sort="number" style="pretty">
                                <column title="id">jube_benchmark_id</column>
                                <column title="wp_id">jube_wp_id</column>
                                <column>host</column>
                                <column>smt</column>
                                <column>taskspernode</column>
                                <column>threadspertask</column>
                                <column>n</column>
                                <column>nb</column>
                                <column>p</column>
                                <column>q</column>
                                <column format=".0f">time</column>
                                <column format=".0f">flops</column>
                        </table>
                        <!--
                        <table name="table.systemparams" sort="number" style="pretty">
                                <column>host</column>
                                <column format=".0f">cpu_bw_avg</column>
                                <column format=".0f">cpu_fp_avg</column>
                                <column format=".0f">h2d_bw_avg</column>
                                <column format=".0f">d2h_bw_avg</column>
                                <column format=".0f">bid_bw_avg</column>
                                <column format=".0f">gpu_bw_avg</column>
                                <column format=".0f">gpu_fp_avg</column>
                                <column format=".0f">net_bw_avg</column>
                                <column format=".0f">net_lat_avg</column>
                        </table>
                        -->
                        <table name="table.progress" sort="number" style="pretty">
                                <column>host</column>
                                <column format=".0f">flops15_avg</column>
                                <column format=".0f">flops25_avg</column>
                                <column format=".0f">flops35_avg</column>
                                <column format=".0f">flops45_avg</column>
                                <column format=".0f">flops55_avg</column>
                                <column format=".0f">flops65_avg</column>
                                <column format=".0f">flops75_avg</column>
                                <column format=".0f">flops85_avg</column>
                                <column format=".0f">flops95_avg</column>
                        <!--
                                <column format=".0f">nleft15_avg</column>
                                <column format=".0f">nleft25_avg</column>
                                <column format=".0f">nleft35_avg</column>
                                <column format=".0f">nleft45_avg</column>
                                <column format=".0f">nleft55_avg</column>
                                <column format=".0f">nleft65_avg</column>
                                <column format=".0f">nleft75_avg</column>
                                <column format=".0f">nleft85_avg</column>
                                <column format=".0f">nleft95_avg</column>
                        -->
                        </table>
                        <table name="table.gpu_monitoring" sort="number" style="pretty" tag="gpu">
                                <column>host</column>
                                <column format=".0f">flops</column>
                                <column title="occurences">warn_clock_cnt</column>
                                <column title="clock_min">warn_clock_min</column>
                                <column format=".0f" title="clock_avg">warn_clock_avg</column>
                                <column title="clock_max">warn_clock_max</column>
                                <column title="power_min">warn_power_min</column>
                                <column format=".0f" title="power_avg">warn_power_avg</column>
                                <column title="power_max">warn_power_max</column>
                                <column title="temp_min">warn_temp_min</column>
                                <column format=".0f" title="temp_avg">warn_temp_avg</column>
                                <column title="temp_max">warn_temp_max</column>
                        </table>
                </result>

                <analyser name="analyser.nmon" reduce="false">
                        <use from="patternsets.xml">patternset.nmon</use>
                        <analyse step="run">
                                <file>*.nmon</file>
                        </analyse>
                </analyser>

                <result>
                        <use>analyser.nmon</use>
                        <table name="table.nmon" sort="number" style="pretty">
                                <column title="id">jube_benchmark_id</column>
                                <column title="wp_id">jube_wp_id</column>
                                <column format=".1f">cpuall_user_avg</column>
                                <column format=".1f">cpuall_sys_avg</column>
                                <column format=".1f">cpuall_wait_avg</column>
                                <column>cpus</column>
                                <column format=".0f">mhz_min</column>
                                <column format=".0f">mhz_avg</column>
                                <column format=".0f">mhz_max</column>
                                <column format=".1f">top_cpu_avg</column>
                                <column format=".0f">memfree_avg</column>
                                <column format=".0f">swapfree_avg</column>
                        </table>
                </result>

                <analyser name="analyser.querygpu" reduce="false" tag="gpu">
                        <use from="patternsets.xml">patternset.querygpu</use>
                        <analyse step="run">
                                <file>*.nmon</file>
                        </analyse>
                </analyser>

                <result tag="gpu">
                        <use>analyser.nmon</use>
                        <table name="table.querygpu" sort="number" style="pretty">
                                <column title="id">jube_benchmark_id</column>
                                <column title="wp_id">jube_wp_id</column>
                                <column format=".0f">flops</column>
                                <column format=".0f">ccg_avg</column>
                                <column format=".0f">ccg_max</column>
                                <column format=".0f">pd_avg</column>
                                <column format=".0f">pd_max</column>
                                <column format=".0f">tg_avg</column>
                                <column format=".0f">tg_max</column>
                                <column format=".0f">ug_avg</column>
                                <column format=".0f">ug_max</column>
                                <column format=".0f">um_avg</column>
                                <column format=".0f">um_max</column>
                        </table>
                </result>
        </benchmark>

        <benchmark name="Build" outpath="~//benchmark/hpl/2.2/var">
                <parameterset name="parameterset.benchmark">
                        <parameter name="arch">ppc64le</parameter>
                </parameterset>

                <fileset name="fileset.benchmark">
                        <prepare>tar --extract --file=${srcpkg} --gunzip --strip-components=1 --verbose</prepare>
                        <prepare>mkdir --parent --verbose ${execprefix}/bin</prepare>
                </fileset>

                <substituteset name="substituteset.benchmark.make">
                        <iofile in="${jube_benchmark_home}/Make.UNKNOWN.in" out="${jube_wp_abspath}/Make.${arch}" />
                        <sub source="@SHELL@" dest="/bin/bash" />
                        <sub source="@CD@" dest="cd" />
                        <sub source="@CP@" dest="cp" />
                        <sub source="@LN_S@" dest="ln -fs" />
                        <sub source="@MKDIR@" dest="mkdir" />
                        <sub source="@RM@" dest="rm -f" />
                        <sub source="@TOUCH@" dest="touch" />
                        <sub source="@ARCH@" dest="${arch}" />
                        <sub source="@TOPDIR@" dest="${jube_wp_abspath}" />
                        <sub source="@MPDIR@" dest="" />
                        <sub source="@MPINC@" dest="" />
                        <sub source="@MPLIB@" dest="" />
                        <sub source="@LADIR@" dest="" />
                        <sub source="@LAINC@" dest="" />
                        <sub source="@LALIB@" dest="${esslsmp_lib} ${xlfortran_lib}" />
                        <sub source="@F2CDEFS@" dest="-DNoChange" />
                        <sub source="@HPL_OPTS@" dest="-DASYOUGO -DASYOUGO2" />
                        <sub source="@CC@" dest="${mpicc}" />
                        <sub source="@CCNOOPT@" dest="${cflags_base}" />
                        <sub source="@CCFLAGS@" dest="${cflags_base} ${cflags_optim_3}" />
                        <sub source="@LINKER@" dest="$(CC)" />
                        <sub source="@LINKFLAGS@" dest="$(CCFLAGS) ${esslsmp_ldflags} ${xlfortran_ldflags}" />
                        <sub source="@ARCHIVER@" dest="${ar}" />
                        <sub source="@ARFLAGS@" dest="${arflags}" />
                        <sub source="@RANLIB@" dest="${ranlib}" />
                </substituteset>

                <step name="build">
                        <use from="platform.xml">parameterset.platform</use>
                        <use>parameterset.global</use>
                        <use>parameterset.benchmark</use>
                        <use>fileset.benchmark</use>
                        <use>substituteset.benchmark.make</use>
                        <do>${make} arch="${arch}"</do>
                        <do>cp --preserve --verbose ${jube_wp_abspath}/bin/${arch}/xhpl ${execprefix}/bin/xhpl-$(printf "%06d" ${jube_benchmark_id})</do>
                </step>
        </benchmark>
</jube>
```
