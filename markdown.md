查看商品分类：预计到时候改为游戏，一级目录先不用
http://malldev.huhp.cc/api/mall-admin/productCategory/list/withChildren
入口方法：com.macro.mall.controller.PmsProductCategoryController#listWithChildren
相关表：pms_product_category

商品品牌，预计隐藏，搞个唯一的，以后再搞成对应的公司，如腾讯，网易等
http://malldev.huhp.cc/api/mall-admin/brand/list?pageNum=1&pageSize=100
入口方法：com.macro.mall.controller.PmsBrandController#getList(java.lang.String, java.lang.Integer, java.lang.Integer)
相关表:pms_brand

可以用的规格(SKU)目录
http://malldev.huhp.cc/api/mall-admin/productAttribute/category/list?pageNum=1&pageSize=100
入口方法：com.macro.mall.controller.PmsProductAttributeCategoryController#getList
table:pms_product_attribute_category

pms_sku_stock
pms_product


根据规格id查规格具体的属性，属性应该是充值方式，区服，礼包那些
http://malldev.huhp.cc/api/mall-admin/productAttribute/list/2?pageNum=1&pageSize=100&type=0
入口方法：com.macro.mall.controller.PmsProductAttributeController#getList
table:pms_product_attribute


广告列表就是轮播图
商品分类的页面下“是否显示”选项决定了分类是否展示它