点击更多的icon时，更新QSM
退出更多，同样更新

##### QuickSwitchManager流程
com.mediatek.camera.common.mode.photo.device.PhotoDeviceController.PhotoDeviceHandler#doOnOpened
```
mSettingManager.getSettingController().addViewEntry();
mSettingManager.getSettingController().refreshViewEntry();
```

 ```
List<ICameraSetting> settingsRelatedToMode = getSettingByModeType(mModeType);
for (ICameraSetting setting : settingsRelatedToMode) {
    if (!access.isValid()) {
        break;
    }
    setting.addViewEntry();
}
···
mAppUi.registerQuickIconDone();
 ```
 将View添加到mQuickSwitcherLayout中
 com.mediatek.camera.ui.QuickSwitcherManager#updateQuickItems
 ```
 private void updateQuickItems() {
     if (mQuickSwitcherLayout != null && mQuickSwitcherLayout.getChildCount() != 0) {
         mQuickSwitcherLayout.removeAllViews();
     }
     //计算权重
     if (mQuickSwitcherLayout != null) {
         LinearLayout.LayoutParams quickSwitcherLP = (LinearLayout.LayoutParams) mQuickSwitcherLayout.getLayoutParams();
         quickSwitcherLP.width = 0;
         int modeIndex = mApp.getAppUi().getModeIndex();
         int moreModeIndex = mApp.getAppUi().isMoreModeItemClickedIndex();
         String vsdof = mApp.getActivity().getResources().getString(R.string.sdof_mode_name);
         if ( modeIndex == ModeSwitchManager.SWITCH_MODE_TIMELAPSE) {
             quickSwitcherLP.weight = 3;
         } else if (modeIndex == ModeSwitchManager.SWITCH_MODE_SLOWMOTION ||
                 modeIndex == ModeSwitchManager.SWITCH_MODE_VIDEO ||
                 //lwl p-5567 begin
                 (moreModeIndex == MoreModeviewUI.ICON_MANAUAL &&
                 !mApp.getAppUi().isInMoreModeUI())) {
                 //lwl p-5567 end
             quickSwitcherLP.weight = 2;
             //lwl add p-5569,p-5037 begin
         }else if ((modeIndex == ModeSwitchManager.SWITCH_MODE_TSREFOCUS ||
                 moreModeIndex == MoreModeviewUI.ICON_NIGHT || //modify for PRODUCTION-6069 :likai 2017.11.09
                 moreModeIndex == MoreModeviewUI.ICON_CALIBRATION ||
                 vsdof.equals(mApp.getAppUi().getCurrentModeName())) &&
                 !mApp.getAppUi().isInMoreModeUI()){
             quickSwitcherLP.weight = 1;
             //lwl add p-5569,p-5037 end
         } else if (modeIndex == ModeSwitchManager.SWITCH_MODE_PHOTO ||
                 modeIndex == ModeSwitchManager.SWITCH_MODE_MORE ||
                 modeIndex == ModeSwitchManager.SWITCH_MODE_BEAUTY ||
                 mApp.getAppUi().isInMoreModeUI()) {
             quickSwitcherLP.weight = 4;
         }
     }
     //addView
     if (mQuickSwitcherLayout != null) {
         Iterator iterator = mQuickItems.entrySet().iterator();
         while (iterator.hasNext()) {
             Map.Entry map = (Map.Entry) iterator.next();
             View view = (View) map.getValue();
             LinearLayout.LayoutParams params = new LinearLayout.LayoutParams(
                     0,
                     ViewGroup.LayoutParams.MATCH_PARENT);
             params.weight = 1.0f;
             view.setLayoutParams(params);
             mQuickSwitcherLayout.addView(view);
         }
         updateViewOrientation();
     }
 }
```
 