# 项目Service原型功能说明：
  poi导入<br />
  导出,支持百万级数据模板导出<br />
  合并excel<br />

## 项目参考
  ExcelUtils http://excelutils.sourceforge.net
  备注：原来源不支持2007excel导出,还有些其他bug，因此在其上面再进行了开发
  
###支持标签
模板语法示例(具体可参考相关模板文件common-poi\src\main\resources\xlsx)
####if

####foreach
<span>
#foreach detail in ${list}  <br />
  ${detail.name}  <br />
#end  <br />
</span>

####如果值为null则设置默认值

#### 开发思路
1.打开工作簿  
2.遍历工作簿的sheet，对每个sheet进行模板导出。
3.读取模板文件，根据模板文件边读取边写入新的文件.(为什么这么做？因为SXSSFWorkbook不支持读数据,只支持写数据)。
4.遇到标签翻译标签值,遇到字符串翻译字符串值，该步骤必须以cell为单位(边读边写入数据)。            
          
#### 测试
支持万条数据缓存模板导出，详见ExcelExpEachTest.main方法，可以手动修改该循环次数,模拟万条数据缓存导出
这个目前有局限性,目前还不支持合并单元格,foreach里面有2行数据的场景，后续可能会考虑优化处理这些问题。
如果导出有需要支持合并单元格、foreach、xls的场景，但对缓存导出无要求，可以看common-poi-nocahe导出。