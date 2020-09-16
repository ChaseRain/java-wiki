## jdk特性

### stream流

```shell
for (ShopSessionProduct sessionProduct : sessionProductList) {
            Long productId = sessionProduct.getProductId();
            int rank = sessionProduct.getRank();
            ShopUserProduct userProduct = new ShopUserProduct();
            userProduct.setSessionId(sessionId);
            userProduct.setUserId(userId);
            userProduct.setRank(rank);
            userProduct.setProductId(productId);

            userProduct.setIsUnlock(1);
            //排序为5或6进行加锁
            if ( rank == 5 || rank == 6){
                userProduct.setIsUnlock(0);
            }
            userProduct.setCreateTime(now);
            userProduct.setUpdateTime(now);
            save(userProduct);
}

# 流式写法
results.stream().map(result -> {
    ShopUserProduct shopUserProduct = BeanUtils.copy(result, ShopUserProduct.class);
    int rank = result.getRank();
    shopUserProduct.setIsUnlock(1);
    //排序为5或6进行加锁
    int isUnlock = (rank == 5 || rank == 6)?0:1;
    shopUserProduct.setIsUnlock(isUnlock);

    shopUserProduct.setUserId(userId);
    shopUserProduct.setCreateTime(now);
    shopUserProduct.setUpdateTime(now);
    save(shopUserProduct);

    return shopUserProduct;
}).collect(Collectors.toList());
```
