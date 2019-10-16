# HEVC-CUpartition-visualization
Visualize HEVC CU partition and TU partition


## Modifications of HM 16.7

Add the code right after `m_pcLoopFilter->loopFilterPic(pcPic);` in TEncGOP.cpp

To generate CU depth string：
```
ofstream myfile_CU;
string CU_file_name = "CU_file_name";
myfile_CU.open(CU_file_name);

for (UInt ctuRsAddr = 0; ctuRsAddr < pcPic->getNumberOfCtusInFrame(); ctuRsAddr++)
{
    TComDataCU* pCtu = pcPic->getCtu(ctuRsAddr);

    const TComSPS &sps = *(pCtu->getSlice()->getSPS());
    const UInt maxCuWidth = sps.getMaxCUWidth();

    int iCount = 0;
    int iWidthInPart = maxCuWidth >> 2;

    for (int i = 0; i < pCtu->getTotalNumPart(); i++)
    {
        myfile_CU << to_string(pCtu->getDepth(g_auiRasterToZscan[i])) + " ";
        iCount++;
    }
}
myfile_CU.close();
```

To generate TU depth string：
```
ofstream myfile_TU;
string TU_file_name = "TU_file_name";
myfile_TU.open(TU_file_name);
for (UInt ctuRsAddr = 0; ctuRsAddr < pcPic->getNumberOfCtusInFrame(); ctuRsAddr++)
{
    TComDataCU* pCtu = pcPic->getCtu(ctuRsAddr);

    const TComSPS &sps = *(pCtu->getSlice()->getSPS());
    const UInt maxCuWidth = sps.getMaxCUWidth();

    int iCount = 0;
    int iWidthInPart = maxCuWidth >> 2;

    for (int i = 0; i < pCtu->getTotalNumPart(); i++)
    {
        int TU_depth = pCtu->getTransformIdx(g_auiRasterToZscan[i]) + pCtu->getDepth(g_auiRasterToZscan[i]);
        myfile_TU << to_string(TU_depth) + " ";
        iCount++;
    }
}
myfile_TU.close();
```
