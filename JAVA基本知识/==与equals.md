== 对于基本类型来说是值比较，对于引用类型来说是比较的是引用；而

equals 默认情况下是引用比较，只是很多类重写了 equals 方法，比如 String、Integer 等把它变成了值比较，所以一般情况下 equals 比较的是值是否相等，如果没有重写equals，默认继承Object的equals方法，比较两个对象的地址是否相同。 