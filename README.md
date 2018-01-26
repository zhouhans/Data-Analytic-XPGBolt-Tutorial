# Data-Analytic-XPGBolt-Tutorial

## Introduction
This tutorial introduces how to launch and use XPGBolt, a PostgreSQL based FPGA accelerating library in AWS EC2 F1 Instance. 

The source code of XPGBolt can be found at [Xilinx Data-Analytic Repo].

Instruction for launching an F1 instance can be found [here].

//Note: Should standardise how the links show up: here vs name of repo/page.

## Steps to Launch XPGBolt

Start by launching two terminals, both to the same F1 instance.

//Note: Should we include information about why the user is running two terminals both to the same instance? What do they do? 

### Terminal 1
1. Navigate to `/home/centos/xpg_demo`.
    ```
    $ cd /home/centos/xpg_demo
    $ ls
    Dockerfile  afi  postgres_f2  run_root.sh
    ```
2. Run `./run_root.sh xpg_demo_container work_space`.
    ```
    $ ./run_root.sh xpg_demo_container work_space
    root@0f077c7d68a7:/# 
    ```
3. Now that you are inside the docker container, navigate to `/home/work_space/demo`.
    ```
    $ cd /home/work_space/demo
    $ ls
    loadfpga.sql  postgres_f2  queries  run_pg.sh
    ```
4. Run `run_pg.sh`.
    ```
    $ ./run_pg.sh
    2018-01-23 22:38:46.530 UTC [21] LOG:  database system was shut down at 2018-01-08 21:45:59 UTC
    2018-01-23 22:38:46.538 UTC [21] LOG:  MultiXact member wraparound protections are now enabled
    2018-01-23 22:38:46.540 UTC [25] LOG:  autovacuum launcher started
    2018-01-23 22:38:46.541 UTC [20] LOG:  database system is ready to accept connections
    ```

### Terminal 2
1. Run `sudo docker exec –it xpg_demo_container /bin/bash`.
    ```
    $ sudo docker exec –it xpg_demo_container /bin/bash
    root@0f077c7d68a7:/# 
    ```
2. Navigate to `/home/work_space/demo`.
    ```
    $ cd /home/work_space/demo
    $ ls
    loadfpga.sql  postgres_f2  queries  run_pg.sh
    ```   
4. Now that you are inside the docker container, run `su postgres` to change user to `postgres`. This takes you inside the PostgreSQL console.
    ```
    $ su postgres
    postgres@0f077c7d68a7:
    ```
6. Run `psql tpch_1g` to connect to 1GB TPCH database.
    ```
    $ psql tpch_1g
    psql (9.6.3, server 9.6.2)
    Type "help" for help.

    tpch_1g=# 
    ``` 
7. Load XPGBolt library at runtime. This step loads the .so file and the FPGA bitstream.
    ```
    tpch_1g=# \i loadfpga.sql
    LOAD
    CREATE FUNCTION
    CREATE FUNCTION
    SET
    tpch_1g=# 
    ```
8. Run TPCH query 6.
    ```
    tpch_1g=# \i queries/6.sql
        revenue    
        ---------------
        45040342.8530
        (1 row)
    ```
9. Turn on timing and rerun query 6 to see the runtime.
    ```
    tpch_1g=# \timing
    tpch_1g=# \i queries/6.sql
    ```
//Note: What does the user do next? Terminal 1 connection ends with being connected. With terminal 2, I'm not sure.
    
[here]: https://github.com/Xilinx/ML-Development-Stack-From-Xilinx/blob/master/launching_instance.md
[Xilinx Data-Analytic Repo]: https://github.com/Xilinx/data-analytics/tree/master/xpg/host



