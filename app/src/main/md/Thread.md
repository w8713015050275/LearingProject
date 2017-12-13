## Thread

### run in deviceController thread
```
    /**
     * Returns the current settings for this Camera service.
     *
     * @return the parameters
     */
    public Parameters getParameters() {
        Parameters[] params = new Parameters[1];
        //切換到API1-Request-0线程
        Message msg = mRequestHandler.obtainMessage(CameraActions.GET_PARAMETERS, params);
        if (mAtomAccessor.sendAtomMessageAndWait(mRequestHandler, msg)) {
            return params[0];
        } else {
            return mRequestHandler.getOriginalParameters();
        }
    }
    ```
    
    
