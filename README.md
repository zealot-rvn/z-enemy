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
