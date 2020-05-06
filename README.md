Z-ENEMY  From: Dk & Enemy (z-enemy)

IMPORTANT:
For maximum performance make sure you have latest drivers 
http://www.nvidia.com/Download/index.aspx

Changes:
- Added kawpow algo (Upcoming Ravencoin hardfork)

_____________________________________
First time or troubleshooting kawpow:
-   First time users - all ver. 2.02+ works on Cuda 9.x &Cuda 10.x and it is recommended to make sure you've updated your NVIDIA drivers. You can find drivers here: http://www.nvidia.com/Download/index.aspx ver., min  ver.398+++
-   Next important thing is intensity. We recommend intensity -19 or 20,(21 for 20x0) at first.
-   kawpow algo using +mem , but use no OC at first until you verify stability.. 

----------------------------- Zealot/Enemy 2.0 HTTP JSON API Reference ----------------------------------------------

Starting with version 2.0 and above, the miner comes with built-in HTTP JSON API server and web control panel.
When the miner is started, the server is automatically started, accessible with your browser at http://127.0.0.1:4067 
(or http://localhost:4067 if you wish). You can specify any external IP address using --api-bind-http=addr:port
If you want to disable it use --api-bind-http=0


There are also few API methods returning json-formatted data available.
1. Shares history: http://localhost:4067/history
2. Current pool info: http://localhost:4067/poolinfo
3. GPU hardware info: http://localhost:4067/hwinfo
4. Summary (optionally includes gpu info and pool info):
       http://localhost:4067/summary?poolinfo=1 (summary with current pool info)
       http://localhost:4067/summary?gpuinfo=1 ( summary with gpu info)
       http://localhost:4067/summary (short summary, no pool or gpu info)	
5. Remote quit (or restart, if you start the miner in the loop): http://localhost:4067/quit

More detailed information about these methods (everything below are GET methods only)

HISTORY (http://localhost:4067/history) - outputs the array of the last 100 shares submitted.  

Output example:

[{"id":1, "gpu_id":0, "height":716954, "hashrate":35269128.19, "ping":793, "difficulty":152577.397563, "ts":1558426499},
{ ... },
...
{ ... }]

Where:
	
	"id":1, 			- share number
	"gpu_id":0,			- GPU ID (the one which found the nonce)
	"height":716954,		- block number
	"hashrate":35269128.19, 	- GPU's speed in hashes per second
	"ping":793,			- pool response time (ping) in milliseconds
 	"difficulty":152577.397563, 	- network difficulty
	"ts":1558426499,		- time, the number of seconds since 1 January 1970 (UNIX timestamp)


POOLINFO (http://localhost:4067/poolinfo) - outputs the data about the current pool session.

Output example:

{"url": "stratum+tcp://pool.minermore.com:4403", "user": "RFAV6gtcwFDfTQkumUgkQ9YBvLW56M8", "solved_count": 0, "accepted_count":513, "rejected_count":1, "job_height":716981, "best_share_diff":180.015057, "ping":233, "retries":0, "last_share":2}

Where:

	"url": "stratum+tcp://rvn.antpool.com:9003"	- information about the pool where the miner is connected
	"user": "RFAV6gtcwFDfTQkumUgkQ9YBvLW56M8",	- the address of your vallet/username
	"solved_count": 0,				- the number of blocks found by the miner
	"accepted_count":513,				- number of shares accepted by the pool
	"rejected_count":1,				- the number of shares rejected by the pool
	"best_share_diff":180.015057			- best share difficulty you have found so far
	"ping":233,					- current ping in milliseconds
	"retries":0,					- the number of connection attempts in case of loss of connection
	"last_share":2					- time in seconds after the last share

HWINFO (http://localhost:4067/hwinfo) - outputs the array with detailed information about the GPUs

Output example:

[{"dev_id":1, "bus_id":5, "name":"MSI GTX 1080 Ti", "vendor":"MSI", "shader_model":610, "memory_size":11264, "intensity":21, "temperature":63, "fan_speed":95, "gpu_clock":1657500, "memory_clock":5505000, "power":226493, "hashrate":30632261},
{ ... },
...
{ ... }]

Where

	"dev_id":1,			- GPU number # in the system
	"name":"MSI GTX 1080 Ti",	- the name of the GPU model
	"vendor":"MSI",			- the manufacturer of the video card
	"shader_model":610,		- unified shader model (6.1)
	"memory_size":11264,		- memory size, in megabytes
	"intensity":21,			- current intensity, set by user
	"temperature":63		- current GPU temperature (Celsius degree)
	"fan_speed":95,			- current speed of the cooling fans, in percent
	"gpu_clock":1657500,		- current GPU clock
	"memory_clock":5505000,		- curent device memory clock
	"power":226493,			- current GPU # energy consumption in milliwatts	
	"hashrate":30632261		- current GPU # speed in hashes per second

	
SUMMARY (http://localhost:4067/summary?poolinfo=1&gpuinfo=1) full and detailed information from the current process of mining
In addition to the information described above, there are also (Example and explanations):

Output example:

{"name":"zealot", "version":"enemy-2.0", "api":"1.0", "algorithm": "x16r", "gpu_count":5, "hashrate": 66369273,
"active_pool": { ... }. "gpuinfo": [{ ... }]}

WHERE

	"name": "zealot",		- miner name
	"version": "enemy-2.0",		- miner version
	"api": "1.0",			- HTTP API version
	"algorithm": "x16r",		- currnet mining algo	
	"gpu_count": 5,			- total number of used GPUs
	"hashrate": 66369273,		- average hashrate for all GPUs
	"active_pool": { ... }          - same as POOLINFO, see above (only included if poolinfo=1)
	"gpus": [{ ... }]               - same as HWINFO, see above (only included if gpuinfo=1)

If you don't want to have gpu info or pool info included, don't set poolinfo and/or gpuinfo to 1 when calling


QUIT (http://localhost:4067/quit)

Allows to remotely quit/restart the miner if command line --api-remote option was specified.

------------------------HELP-----------------------

Usage: z-enemy [OPTIONS]
Options:
  -a, --algo=ALGO           Coin hash algorithm to use:
					aergo		(AeriumX: AEX)
					bitcore		(Bitcore: BTX)
					bcd		(Bitcoin Diamond: BCD)
					kawpow		(KawPow: Ravencoin)
					x16r		(X16R: old Ravencoin)
					x16rv2		(X16Rv2: old Ravencoin)
					x16s		(X16S: Pigeon)
					x17		(X17: Verge)
					c11		(C11: CHC)
					phi		(PHI1612: Folm, Seraph)
					phi2		(PHI2: LUXCoin)
					tribus		(Tribus: Denarius)
					poly		(Poly: Polytimos)
					skunk		(Skunk: Skunk)
					sonoa		(Sonoa: SONO)
					hex		(Hex: HEX)
					timetravel	(Machinecoin: MAC)
					xevan		(Xevan: Transend)
  -d, --devices             Comma separated list of CUDA devices to use (0,1 etc).
                            Alternatively takes
                            string names of your cards like MSI 1080 Ti or MX150#2
                            (matching 2nd gt640 in the PC)
  -i  --intensity=N[,N]     GPU intensity 8.0-31.0, decimals allowed (default: 19) 
      --cuda-schedule       set CUDA scheduling option:
	                           0: BlockingSync (default)
	                           1: Spin
	                           2: Yield
  -f, --diff-factor         Divide difficulty by this factor (default 1.0) 
  -l, --log=FILE            Duplicate output into log file. Sample: --log=logfile.txt
  -m, --diff-multiplier     Multiply difficulty by this value (default 1.0) 
  -o, --url=URL             URL of mining server
  -O, --userpass=U:P        username:password pair for mining server
  -u, --user=USERNAME       username for mining server
  -p, --pass=PASSWORD       password for mining server
  -x, --proxy=[PROTOCOL://]HOST[:PORT]  connect through a proxy
  -r, --retries=N           number of times to retry if a network call fails
                            (default: retry indefinitely)
  -R, --retry-pause=N       time to pause between retries, in seconds (default: 30)
  -T, --timeout=N           network timeout, in seconds (default: 300)
  -s, --scantime=N          upper bound on time spent scanning current work when
                            long polling is unavailable, in seconds (default: 60)
  -n, --ndevs               list cuda devices
  -N, --statsavg            number of samples used to compute hashrate (default: 30)
      --no-extranonce       disable extranonce subscribe on stratum
  -q, --quiet               disable per-thread hashmeter output
      --no-cert-verify      disable stratum+ssl certificate verification
      --no-color            disable colored output
      --no-nvml             disable NVML hardware sampling
      --no-reconnect        ignore client.reconnect requests
      --cpu-affinity        set process affinity to cpu core(s), mask 0x3 for cores 0 and 1
      --cpu-priority        set process priority (default: 3) 0 idle, 2 normal to 5 highest
  -b, --api-bind=port       IP:port for the legacy API (default: 127.0.0.1:4068), 0 disabled
      --api-allow=...       IP/mask of the allowed api client(s), 0/0 for all
      --api-remote          Allow remote control, like pool switching or exiting
      --api-bind-http=port  IP:port for the HTTP API (default: 127.0.0.1:4067), 0 disabled
      --max-temp=N          Only mine if gpu temperature is less than specified value
                            Can be tuned with --resume-temp=N to set a resume value
  -B, --background        run the miner in the background
  -c, --config=FILE       load a JSON-format configuration file Sample: --config=config.json
  -V, --version           display version information and exit
  -h, --help              display this help text and exit


