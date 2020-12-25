---
title: Implementing the demo
description: 15
---
<p><strong>1.Create an instance from HwAssetManager. We will use this instance for all asset operations</strong></p>
<pre><div id="copy-button10" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>  private val hwAssetManager = HwAssetManager.getInstance()   
</code></pre>



<p><strong>2. Handling Error Codes</strong></p>
<pre><div id="copy-button15" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>  private fun onFailure(result : HwAssetManager.AssetResult,operationTitle :String) {
        when(result.resultCode) {
            HwAssetManager.ERROR_CODE_INVALID_ARGUMENT -> {
                Log.e(operationTitle,"Invalid Arguments"+" errorCode: " + HwAssetManager.ERROR_CODE_INVALID_ARGUMENT )
                Toast.makeText(context,"Invalid Arguments Error ", Toast.LENGTH_SHORT).show()
            }
            HwAssetManager.ERROR_CODE_PERMISSION_DENIED -> {
                Log.e(operationTitle,"Permission Denied" +" errorCode: " + HwAssetManager.ERROR_CODE_PERMISSION_DENIED )
                Toast.makeText(context,"Invalid Arguments Error ", Toast.LENGTH_SHORT).show()
            }
            HwAssetManager.ERROR_CODE_UNINITIALIZED -> {
                Log.e(operationTitle,"Initialization error" +" errorCode: " + HwAssetManager.ERROR_CODE_UNINITIALIZED )
                Toast.makeText(context,"Initialization Error ", Toast.LENGTH_SHORT).show()
            }
            HwAssetManager.ERROR_CODE_DATABASE_ERROR -> {
                Log.e(operationTitle,"Database operation error" +" errorCode: " + HwAssetManager.ERROR_CODE_DATABASE_ERROR )
                Toast.makeText(context,"Database Operation Error ", Toast.LENGTH_SHORT).show()
            }
            HwAssetManager.ERROR_CODE_UNSECURED_ENVIRONMENT -> {
                Log.e(operationTitle,"unsecured environment error" +" errorCode: " + HwAssetManager.ERROR_CODE_UNSECURED_ENVIRONMENT )
                Toast.makeText(context,"Unsecured Environment Error ", Toast.LENGTH_SHORT).show()
            }
            HwAssetManager.ERROR_CODE_SYSTEM_ERROR -> {
                Log.e(operationTitle,"System error" +" errorCode: " + HwAssetManager.ERROR_CODE_SYSTEM_ERROR )
                Toast.makeText(context,"System Error ", Toast.LENGTH_SHORT).show()
            }
        }
    }
</code></pre>

<p><strong>3.Asset Insert</strong></p>
<pre><div id="copy-button11" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>  fun insertData(bundle: Bundle){
        val result  = hwAssetManager.assetInsert(context, bundle)
        if (result.resultCode == HwAssetManager.SUCCESS) {
            Toast.makeText(context,"Your asset successfully inserted: " + result.resultInfo[0], Toast.LENGTH_SHORT).show()
        }else
            onFailure(result,"Asset Insert")
    }
</code></pre>

<p><strong>4.Asset Update</strong></p>
<pre><div id="copy-button12" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>  fun updateData (bundle: Bundle) {
        val result  = hwAssetManager.assetInsert(context, bundle)
        if (result.resultCode == HwAssetManager.SUCCESS) {
            Toast.makeText(context,"Your asset successfully updated: " + result.resultInfo[0], Toast.LENGTH_SHORT).show()
        }else
            onFailure(result,"Asset Update")
    }
</code></pre>

<p><strong>5.Asset Delete</strong></p>
<pre><div id="copy-button14" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>  fun deleteData(bundle: Bundle) {
        val result = hwAssetManager.assetDelete(context,bundle)
         if (result.resultCode == HwAssetManager.SUCCESS){
             Toast.makeText(context,"Your asset successfully deleted: " + result.resultInfo[0], Toast.LENGTH_SHORT).show()
        }else {
             onFailure(result, "Asset Delete")
        }
    }
</code></pre>

<p><strong>6.Asset Select</strong></p>
<pre><div id="copy-button13" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>  fun selectData(bundle: Bundle) :ArrayList<AssetModel>{
        val result = hwAssetManager.assetSelect(context,bundle)
        if (result.resultCode == HwAssetManager.SUCCESS) {
            Toast.makeText(context,"Your asset successfully selected: " + result.resultInfo[0], Toast.LENGTH_SHORT).show()
            val gson = Gson()
            val list: ArrayList<AssetModel> = arrayListOf()
            for (i in result.resultInfo) {
                list.add(gson.fromJson(i, AssetModel::class.java))
            }
            return list
        }else {
            onFailure(result,"Asset Select")
        }    
      return arrayListOf()
    }
