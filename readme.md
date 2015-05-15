##Fuzzy logic implements

Implementation of fuzzy logic in Intel Galileo&reg; platform by @kalmik.

###Features

* FuzzyThread, running the fuzzy routine
* ServerThread, serving the fuzzy output on socket TCP

###Testing

####Time Analysis

fuzzy system with 7 membership functions ( MFS ) and 49 production rules and websockets server.

![Timing Analysis](/C/benchmarks/outputs/timing-teste/7MFS-with-ws.png)

####Graphical comparison

With MATLAB&reg; toolbox.
![Timing Analysis](/C/benchmarks/outputs/plots/comparativo1Matlab.png)
With C routine
![Timing Analysis](/C/benchmarks/outputs/plots/comparativo1Matlab.png)

###How to use

####Code Example 

 * ADD MFS

	```C
	float pGN[4] = {-1,-1,-0.75,-0.5};
	float pMN[3] = {-0.75,-0.5,-0.25};
	float pPN[3] = {-0.5,-0.25,0};
	float pZZ[3] = {-0.25,0,0.25};
	float pPP[3] = {0,0.25,0.5};
	float pMP[3] = {0.25,0.5,0.75};
	float pGP[4] = {0.5,0.75,1,1};
	
	pertinence GN = {&pGN,4};
	pertinence MN = {&pMN,3};
	pertinence PN = {&pPN,3};
	pertinence ZZ = {&pZZ,3};
	pertinence PP = {&pPP,3};
	pertinence MP = {&pMP,3};
	pertinence GP = {&pGP,4};
	```
	
* ADD Rule

	```C
	rule rules[49] = {
	{andOp(fuzzify(e,GN),fuzzify(ed,GN)), &GN},
	{andOp(fuzzify(e,GN),fuzzify(ed,MN)), &GN},
	{andOp(fuzzify(e,GN),fuzzify(ed,PN)), &MN},
	{andOp(fuzzify(e,GN),fuzzify(ed,ZZ)), &MN},
	{andOp(fuzzify(e,GN),fuzzify(ed,PP)), &PN},
	{andOp(fuzzify(e,GN),fuzzify(ed,MP)), &PN},
	{andOp(fuzzify(e,GN),fuzzify(ed,GP)), &ZZ},
	
	{andOp(fuzzify(e,MN),fuzzify(ed,GN)), &GN},
	{andOp(fuzzify(e,MN),fuzzify(ed,MN)), &MN},
	{andOp(fuzzify(e,MN),fuzzify(ed,PN)), &MN},
	{andOp(fuzzify(e,MN),fuzzify(ed,ZZ)), &PN},
	{andOp(fuzzify(e,MN),fuzzify(ed,PP)), &PN},
	{andOp(fuzzify(e,MN),fuzzify(ed,MP)), &ZZ},
	{andOp(fuzzify(e,MN),fuzzify(ed,GP)), &PP},
	
	{andOp(fuzzify(e,PN),fuzzify(ed,GN)), &MN},
	{andOp(fuzzify(e,PN),fuzzify(ed,MN)), &MN},
	{andOp(fuzzify(e,PN),fuzzify(ed,PN)), &PN},
	{andOp(fuzzify(e,PN),fuzzify(ed,ZZ)), &PN},
	{andOp(fuzzify(e,PN),fuzzify(ed,PP)), &ZZ},
	{andOp(fuzzify(e,PN),fuzzify(ed,MP)), &PP},
	{andOp(fuzzify(e,PN),fuzzify(ed,GP)), &PP},
	
	{andOp(fuzzify(e,ZZ),fuzzify(ed,GN)), &MN},
	{andOp(fuzzify(e,ZZ),fuzzify(ed,MN)), &PN},
	{andOp(fuzzify(e,ZZ),fuzzify(ed,PN)), &PN},
	{andOp(fuzzify(e,ZZ),fuzzify(ed,ZZ)), &ZZ},
	{andOp(fuzzify(e,ZZ),fuzzify(ed,PP)), &PP},
	{andOp(fuzzify(e,ZZ),fuzzify(ed,MP)), &PP},
	{andOp(fuzzify(e,ZZ),fuzzify(ed,GP)), &MP},
	
	{andOp(fuzzify(e,PP),fuzzify(ed,GN)), &PN},
	{andOp(fuzzify(e,PP),fuzzify(ed,MN)), &PN},
	{andOp(fuzzify(e,PP),fuzzify(ed,PN)), &ZZ},
	{andOp(fuzzify(e,PP),fuzzify(ed,ZZ)), &PP},
	{andOp(fuzzify(e,PP),fuzzify(ed,PP)), &PP},
	{andOp(fuzzify(e,PP),fuzzify(ed,MP)), &MP},
	{andOp(fuzzify(e,PP),fuzzify(ed,GP)), &MP},
	
	{andOp(fuzzify(e,MP),fuzzify(ed,GN)), &PN},
	{andOp(fuzzify(e,MP),fuzzify(ed,MN)), &ZZ},
	{andOp(fuzzify(e,MP),fuzzify(ed,PN)), &PP},
	{andOp(fuzzify(e,MP),fuzzify(ed,ZZ)), &PP},
	{andOp(fuzzify(e,MP),fuzzify(ed,PP)), &MP},
	{andOp(fuzzify(e,MP),fuzzify(ed,MP)), &MP},
	{andOp(fuzzify(e,MP),fuzzify(ed,GP)), &GP},
	
	{andOp(fuzzify(e,GP),fuzzify(ed,GN)), &ZZ},
	{andOp(fuzzify(e,GP),fuzzify(ed,MN)), &PP},
	{andOp(fuzzify(e,GP),fuzzify(ed,PN)), &PP},
	{andOp(fuzzify(e,GP),fuzzify(ed,ZZ)), &MP},
	{andOp(fuzzify(e,GP),fuzzify(ed,PP)), &MP},
	{andOp(fuzzify(e,GP),fuzzify(ed,MP)), &GP},
	{andOp(fuzzify(e,GP),fuzzify(ed,GP)), &GP}
	};
	```



###Settings

 - AND operation uses MIN
 - OR operation uses MAX
 - IMPLICATION operation uses PRODUCT
 - AGGREGATION operation uses SUM
 - DEFUZZIFY operation uses CENTROID
