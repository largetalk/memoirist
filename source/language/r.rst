R
===================

Rstudio, CRAN, rJava, Rserve, shiny

::
  
  n <- 15
  n <- 1 + rnorm(1)
  ls(); ls.str(pat="M", max.level=-1)
  ?lm
  mode(n)
  length(n)

对象类别概览：向量， 因子， 数组， 矩阵， 数据框， 时间序列， 列表

::

  mydata <- read.table("data.dat")
  read.csv
  read.csv2
  read.delim
  read.delim2
  mydata <- scan()
  write.table

::

  x < 1:30
  seq(1, 5, 0.5)
  seq(length=9, from=1, to=5)
  rep(1, 30) //1 重复30次
  sequence(4:5) // 1 2 3 4 1 2 3 4 5
  gl(2, 6, label=c('F', 'M'))
  expand.grid() 

::

  rnorm(n, mean=0, sd=1)
  rexp(n, rate=1)
  rgamma(n, shape, scale=1)
  rpois(n, lambda)
  rweibull(n, shape, scale=1)
  rcauchy(n, location=0, scale=1)
  rbeta(n, shape1, shape2)
  rt(n, df)
  rf(n, df1, df2)
  rchisq(n, df)
  rbinom(n, size, prob)
  rmultinom(n, size, prob)
  rgeom(n, prob)
  rhyper(nn, m, n, k)
  rlogis(n, location=0, scale=1)
  rlnorm(n, meanlog=0, sdlog=1)
  rnbinom(n, size, prob)
  runif(n, min=0, max=1)
  rwilcox(nn, m, n), rsignrank(nn, n)

rfunc(n, p1, p2...) 可以被替换成dfunc(x,...) (密度函数), pfunc(x,...) (累计概率密度函数), qfunc(x,...) (分布函数)

::

  vector(mode="logical", length=5L)
  factor(1:3, labels=c("A", "B", "C"))
  matrix(data = NA, nrow = 1, ncol = 1, byrow = FALSE, dimnames = NULL)
  x <- 1:4; n <- 10; data.frame(x, n)
  list(x, y)
  ts(data = NA, start = 1, end = numeric(0), frequency = 1, deltat = 1, ts.eps = getOption("ts.eps"), class, names)

%% 模 %/% 整除 xor(x, y) 异或

::

  identical(x, y) //严格相等
  all.equal(x, y) //近似相等

::

  x <- c(1:3); names(x) <- c('a','b','c')
  x <- matrix(1:4, 2); rownames(x) <- c('a', 'b'); colnames(x) <- c('c', 'd'); dimnames(x)

::

  sum(x)
  prod(x) //连乘
  max(x)
  min(x)
  which.max(x) //返回最大元素的下标
  which.min(x)
  range(x)
  length(x)
  mean(x)
  median(x)
  var(x) or cov(x) //方差或协方差
  cor(x) //相关系数矩阵
  var(x, y) or cov(x, y)
  cor(x, y) //相关系数
  round(x, n)
  rev(x) //取逆序
  sort(x)
  rank(x) //元素的秩
  log(x, base) //以base为底的对数值
  scale(x) //中心化和标准化
  pmin(x,y,...)
  pmax(x,y,...)
  cumsum(x) //返回一个向量,它的第i个元素是从x[1]到x[i]的和
  cumprod(x)
  cummin(x)
  cummax(x)
  match(x, y)
  which(x == a)
  choose(n, k) //计算从n个样本中选取k个的组合数
  na.omit(x) //忽略有缺失值(NA)的观察数据
  na.fail(x)
  unique(x)
  table(x)
  table(x, y)
  subset(x, ...)
  sample(x, size)

::

  m1 <- matrix(1, 2, 2)
  m2 <- matrix(2, 2,2)
  rbind(m1, m2)
  cbind(m1, m2)
  m1 * m2 //每个对应元素相乘
  m1 %*% m2 //矩阵相乘
  t(x) //矩阵转置
  diag(x) //提取对角元

::

  plot(x)
  plot(x, y)
  sunflowerplot(x,
  y)
  pie(x)
  boxplot(x) 
  stripchart(x)
  coplot(x~y | z)
  interaction.plot
  (f1, f2, y)
  matplot(x,y)
  dotchart(x)
  fourfoldplot(x)
  assocplot(x)
  mosaicplot(x)
  pairs(x)
  plot.ts(x)
  ts.plot(x)
  hist(x)
  barplot(x)
  qqnorm(x)
  qqplot(x, y)
  contour(x, y, z)
  filled.contour (x,
  y, z)
  image(x, y, z)
  persp(x, y, z)
  stars(x)
  symbols(x, y, ...)
  termplot(mod.obj)
