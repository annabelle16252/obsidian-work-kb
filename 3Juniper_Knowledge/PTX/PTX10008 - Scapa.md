
# Fabric
Scapa SIB 8 (SIB-JNP10008): 3 ZF ASICs

Scapa Line card consisting of 5 BT ASICs, Two DPs, each DP having 36 serdes running at 53.125G

Fabric Connectivity
Each DP has 36 serdes connected to 6 SIBs
6 serdes per SIB, or 2 serdes per ZF
Each ZF is configured as 2 virtual planes

Scapa 里的 BT PFE 有 36 条 fabric links：
系统做成 36 fabric planes，每个 plane 放 1 条 link