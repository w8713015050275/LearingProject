####
Modes.h (d:\gm12b\vendor\mediatek\proprietary\hardware\mtkcam\include\mtkcam\def)	6037	2017/11/1
```
/**
 * @enum EShotMode
 * @brief Shot Mode Enumeration.
 *
 */
enum EShotMode
{
    eShotMode_NormalShot,                           /*!< Normal Shot */
    eShotMode_ContinuousShot,                       /*!< Continuous Shot Ncc*/
    eShotMode_ContinuousShotCc,                     /*!< Continuous Shot Cc*/
    eShotMode_BestShot,                             /*!< Best Select Shot */
    eShotMode_EvShot,                               /*!< Ev-bracketshot Shot */
    eShotMode_SmileShot,                            /*!< Smile-detection Shot */
    eShotMode_HdrShot,                              /*!< High-dynamic-range Shot */
    eShotMode_AsdShot,                              /*!< Auto-scene-detection Shot */
    eShotMode_ZsdShot,                              /*!< Zero-shutter-delay Shot */
    eShotMode_FaceBeautyShot,                       /*!<  Face-beautifier Shot */
    eShotMode_Mav,                                  /*!< Multi-angle view Shot */
    eShotMode_Autorama,                             /*!< Auto-panorama Shot */
    eShotMode_MultiMotionShot,                      /*!< Multi-motion Shot */
    eShotMode_Panorama3D,                           /*!< Panorama 3D Shot */
    eShotMode_Single3D,                             /*!< Single Camera 3D Shot */
    eShotMode_EngShot,                              /*!< Engineer Mode Shot */
    eShotMode_VideoSnapShot,                        /*!< Video Snap Shot */
    eShotMode_FaceBeautyShotCc,                     /*!< Video Snap Shot */
    eShotMode_ZsdHdrShot,                           /*!< ZSD with High-dynamic-range Shot */
    eShotMode_ZsdMfllShot,                          /*!< ZSD with MFLL shot */
    eShotMode_MfllShot,                             /*!< Mfll Shot, only used in middleware v1.4 and later */
    eShotMode_ZsdVendorShot,                        /*!< Zero-shutter-delay Vendor Shot */
    eShotMode_VendorShot,                           /*!< Vendor Shot */
    eShotMode_BMDNShot,                             /*!< Dual cam denoise shot */
    eShotMode_MFHRShot,                             /*!< Dual cam multi-frame high-resoulation shot */
    eShotMode_Undefined = 0xFF,                     /*!< Undefined shot, need to check the use flow */
};
```

```
rpShot = createInstance_ArcBeautyShot("ArcBeautyShot", u4ShotMode, i4OpenId, pParamsMgr);
```
ArcBeautyShot.cpp (d:\gm12b\vendor\mediatek\proprietary\hardware\mtkcam\middleware\v1\adapter\scenario\shot\arcbeautyshot)	54371	2017/12/6
```
extern
sp<IShot>
createInstance_ArcBeautyShot(
    char const*const    pszShotName,
    uint32_t const      u4ShotMode,
    int32_t const       i4OpenId,
    sp<IParamsManager>  pParamsMgr
)
{
    sp<IShot>  pShot            = NULL;
    sp<ArcBeautyShot>  pImpShot = NULL;
    //
    //  (1.1) new Implementator.
    pImpShot = new ArcBeautyShot(pszShotName /*ArcBeautyShot*/, u4ShotMode, i4OpenId, pParamsMgr);
    if  ( pImpShot == 0 ) {
        CAM_LOGE("[%s] new ZsdVendorShot", __FUNCTION__);
        goto lbExit;
    }
    //
    //  (1.2) initialize Implementator if needed.
    if  ( ! pImpShot->onCreate() ) {
        CAM_LOGE("[%s] onCreate()", __FUNCTION__);
        goto lbExit;
    }
    //
    //  (2)   new Interface.
    pShot = new IShot(pImpShot);
    if  ( pShot == 0 ) {
        CAM_LOGE("[%s] new IShot", __FUNCTION__);
        goto lbExit;
    }
    //
lbExit:
    //
    //  Free all resources if this function fails.
    if  ( pShot == 0 && pImpShot != 0 ) {
        pImpShot->onDestroy();
        pImpShot = NULL;
    }
    //
    return  pShot;
}
```