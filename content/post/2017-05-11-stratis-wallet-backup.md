---
title: "stratis wallet backup하기"
metaAlignment: center
date: 2017-05-09
categories:
- blockchain
- cryptocurency
tags:
- stratis
- blockchain
- cryptocurrency
- wallet
- linux
thumbnailImage: //bittrexblobstorage.blob.core.windows.net/public/74083684-8817-4e0e-9107-7fcc129f4e73.png
thumbnailImagePosition: left
---

<!--more-->
![](https://steemitimages.com/0x0/https://cdn-images-1.medium.com/max/800/1*lnHRa3D7XxjCp4Fl7hPgHQ.jpeg)

안녕하세요.

지난글에서 지갑을 무사히 설치했다면 이번글에선 지갑을 백업하는법을 알아보겠습니다.




지갑을 무사히 설치하였더라도, 만일의 사태는 발생할 수 있습니다. 지갑이 설치된 컴퓨터가 파손된다면 어떻게 할건가요?

이럴때를 대비해 지갑을 백업해둬야만 합니다. 관리부주의로 가상화폐의 소유권을 잃어버릴수도 있습니다. 이 글을 읽으시는 분들도 반드시 백업파일을 만들어두시길 바랍니다.

# 백업방법 1

![](https://lh3.googleusercontent.com/_RsYsgIQcLJApNkjqdiQxX8Jgyj4ZnDW-BvcB8J5aHjj74nvqxgf4o-VX_fQtZ8bJx8Gt_Hm08RWwWawd1MVRAy0z7Q1ThT-wE30-1Y0VUzhZYMtztxRpPTZwZ21dtptgxxHLnYUq5IPhIKiUF9NcsAqYN7lPtK0WQ5wm2uamfyt9enM2ve4OuQqmdMcRfwnOVxWZNbwiNElIDQ870Atzp15It_kXlPtsI1fgCzFia3hjucgwpOoieI5VR1ticn8M-esUHpmFdH3kOdN7cIbtpqgk0LC5EtK5asfb4OxxXsko83anACEMOukaNN7ur-PaLUjqNGUUvNJzf48f5zYEyyE8vJaPmJdgMQt0wb-f1snXph1XWQyh0Xlxa3kWb9PEjAGZ319YM3apswP3lipa8hxpBdg1hM_gyJnq12aRid4b-Mjuq05kHoze3N9RGiHWUNoUWMMnKotY_sTTLjZRuscnNJJrzKxCx_IbDrX4qQypL3v0f44pHRHxEIP5G7YBJ802fSha_o8CjTA7fkt-ftFrPLKBA3As9vNIsHQ-zBMxr2YQjEAPL5eNEuVa7iMeEKNW06swNqA7dEcI8hSab_VLivPxjJgf7QCEC3rpEQR3A6wnEqwDECo2248qqhX9YcNP-ZOdc86lAj1KT3XCAoXA0Rw4CiWoMOP=w796-h414-no)
-   간단하게 `파일 -> 지갑 백업`을 통해 백업 파일을 만들 수 있습니다.

![](https://lh3.googleusercontent.com/144qg04MUwMxC8OSyrHUK6M2QVp9iXZNKTB_e4jECPvCSjStjv5TUX5UQtSKjKij3t9g0wv1H0y5gNdfxcx1yTTE66IGmahcTdwhtEQhJUumOyzcqTPs8ToqML1d4H0EEWeih449KNnnvfzymeppdHLqIkv-35SoVtyIDNb_Ds4g6o79xquqnvsF7a60XXgvR3lUHPMCuwfOGiY9UoyeaLDLFG0o8niJTuGkpoKX2Y8tHggAurd6tryXtoQdDk97p_z_opaoZC8jjxYf41vZiGEGjb7BPrPqqQbsOQnNvsuE30gH1cVTgIWV9Ah7VoaHPVgZNtD8WowupQn89FfPMgsPFC3Ivxk4y6CHaYcQieRqRGEmyjvxn7kap-XkV40qvCL7lcbQNHK8jRzAOzd3A2h4AgxVum9iXC3fQY3F5qq4B70CpkO5BHfpF9ivURRNtwiFb0qatEXlNP1U6cfdlkwH0x7qjYlITjWUFOk4_BUpExZvY2qsBHzFn_EKJys84RSDtIdsLALa2Z_5zauwAVZK3XxSc8CuaOYlJRL3cMiZ76u5BRSua-mCFHYGxh-RjLjQv5mTzS8M9A5CZRnXdxCDzwPLVYIvtSVGS49QPuaafY3_cbLtx--ds7SJf8VFJ6s341JNbL3vzEmBAHYTHi0diIbK43OP8jnq=w1410-h1178-no)
-   원하는 경로에 원하는 이름으로 백업 파일을 만들수 있습니다.
-   이름은 원하는대로 설정 가능하지만 일반적으로는 `wallet`으로 하는걸 추천드립니다.(복원할떄 파일 이름이 `wallet.dat`이어야 복원가능함)
-   저는 **백업파일을 압축하여 암호를 걸어둔 뒤** USB, 그리고 클라우드에 보관중입니다.

# 백업방법 2
![](https://lh3.googleusercontent.com/CzlwtUxbH5_yaT6N1Y__yR8tK0xgefA3-a9tCgpDG-9Q2HIsLapYB-e06zEdoTC8keWF2fDX1p3Bqqihch1BAjcDtAGIs2Foc7FBH1Pyx1E0My6OIegSK_S0iSNzYAFQUL3bS8PTtEYffC6ASJlKh7u9QLvnj_dwBJnh7AvmSWdWG37M0iV55yXk2Al8-ZgWv7rv__jsPofxaj1zpDUVmIndzIpuStGs7lj8oamU1C14RFLfm0p-D8K1cgspu_HlRGic5VHWBl1FkwIs-uHgLtlWwpY0-geUMtxLSVhgaOLKk8OHG4yPgZ2zWHpAYWTgzVJQCknuKcIN3N47d3bKbeY6N3OzasNr_AJmg14L13usxQHTumqyZQOEVsSD8NbxvGoFufu5o1X4iplmY9ScnLyWDDccul8NB6bd_GgZf2PpggyYFtk_zFpWKOXvi5RSfT-6daa02tHqNIQbY5D_EiHf2JzCPLx0WZueYYKCE5afSeVSyW1Jy8M_yg6yFHHu5Wf_f4yFOdC4pAr7HBVGqn0tXTvJuxpMV0etiA0CIwf1qsZ1bgFFIo-ZfBbHHJAGJM8Hlz0-AmuiYZbZ90gR2renMl5y8pHGcQyc1rid5VMP0zgNzsXKGAKJauCyIs9cYggsxff-FZQ8958zi3sOa-L11aHUDZqCoPBo=w2276-h1230-no)
-   위의 방법으로 `wallet.dat`파일을 생성해도 되지만 스트라티스 지갑을 설치하셨다면 이미 `wallet.dat`파일이 생성되어있습니다. 이 파일을 복사해서 백업파일로 사용하셔도 됩니다.
-   **각 운영체제별로 wallet.dat 경로가 다릅니다.** 아래의 내용을 참조해주세요.
-   윈도우의 경우, 예를 들어 로그인계정이 `sanghun`이라면 `wallet.dat`의 경로는 *C:\Users\sanghun\AppData\Roaming\Stratis\wallet.dat*입니다.

```
Windows: C:\Users\<로그인한유저명>\AppData\Roaming\Stratis\wallet.dat
Mac: ~/Library/Application Support/stratis/wallet.dat
Linux:  ~/.stratis/wallet.dat
```

# 복구방법
-   컴퓨터를 포맷해서 지갑이 날라갔거나, 지갑을 다른곳으로 옮기고싶을떄 위에서 만들어둔 백업파일을 사용하면 됩니다.
-   백업파일을 아래의 경로에 넣어주세요.

```
Windows: C:\Users\<로그인한유저명>\AppData\Roaming\Stratis\wallet.dat
Mac: ~/Library/Application Support/stratis/wallet.dat
Linux:  ~/.stratis/wallet.dat
```



생각보다 쉽죠? 이게 끝입니다. 간단한 방법으로 이렇게 가상화폐를 지킬수 있습니다. 최근 거래소를 타겟으로한 ddos공격이 늘고 있습니다. 혹시 거래소에 스트라티스를 두고 계시다면 이번 기회에 꼭 개인지갑을 생성해서 개인지갑에 보관하는걸 추천드립니다.
