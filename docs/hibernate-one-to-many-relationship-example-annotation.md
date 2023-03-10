# hibernate–一对多示例(注释)

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-one-to-many-relationship-example-annotation/>

在本教程中，它将重用之前的“ [Hibernate 一对多关系示例 XML 映射](http://web.archive.org/web/20210507115125/http://www.mkyong.com/hibernate/hibernate-one-to-many-relationship-example/)”教程的整个基础设施，并对其进行增强以支持 Hibernate / JPA 注释。

## 项目结构

查看本教程的新项目结构。

![one to many annotation folder](img/8913f87ef8868487c90a0abcf71ddc9a.png "one-to-many-annotation-relationship-folder")**Note**
Since Hibernate 3.6, annotation codes are merged into the Hibernate core module, so, the [previous pom.xml](http://web.archive.org/web/20210507115125/http://www.mkyong.com/hibernate/hibernate-one-to-many-relationship-example/) file can be reuse.

## 1.“一对多”表关系

再看前面的一对多表关系。

![one to many table relationship](img/b63b5fb7484cc753f46b53cc053cce4e.png "one-to-many-relationship")

## 2.Hibernate 模型类

更新以前的模型类—`Stock.java`和`StockDailyRecord.java`，并在里面定义注释代码。

*文件:Stock.java*

```java
 package com.mkyong.stock;

import java.util.HashSet;
import java.util.Set;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.GeneratedValue;
import static javax.persistence.GenerationType.IDENTITY;
import javax.persistence.Id;
import javax.persistence.OneToMany;
import javax.persistence.Table;
import javax.persistence.UniqueConstraint;

@Entity
@Table(name = "stock", catalog = "mkyongdb", uniqueConstraints = {
		@UniqueConstraint(columnNames = "STOCK_NAME"),
		@UniqueConstraint(columnNames = "STOCK_CODE") })
public class Stock implements java.io.Serializable {

	private Integer stockId;
	private String stockCode;
	private String stockName;
	private Set<StockDailyRecord> stockDailyRecords = new HashSet<StockDailyRecord>(
			0);

	public Stock() {
	}

	public Stock(String stockCode, String stockName) {
		this.stockCode = stockCode;
		this.stockName = stockName;
	}

	public Stock(String stockCode, String stockName,
			Set<StockDailyRecord> stockDailyRecords) {
		this.stockCode = stockCode;
		this.stockName = stockName;
		this.stockDailyRecords = stockDailyRecords;
	}

	@Id
	@GeneratedValue(strategy = IDENTITY)
	@Column(name = "STOCK_ID", unique = true, nullable = false)
	public Integer getStockId() {
		return this.stockId;
	}

	public void setStockId(Integer stockId) {
		this.stockId = stockId;
	}

	@Column(name = "STOCK_CODE", unique = true, nullable = false, length = 10)
	public String getStockCode() {
		return this.stockCode;
	}

	public void setStockCode(String stockCode) {
		this.stockCode = stockCode;
	}

	@Column(name = "STOCK_NAME", unique = true, nullable = false, length = 20)
	public String getStockName() {
		return this.stockName;
	}

	public void setStockName(String stockName) {
		this.stockName = stockName;
	}

	@OneToMany(fetch = FetchType.LAZY, mappedBy = "stock")
	public Set<StockDailyRecord> getStockDailyRecords() {
		return this.stockDailyRecords;
	}

	public void setStockDailyRecords(Set<StockDailyRecord> stockDailyRecords) {
		this.stockDailyRecords = stockDailyRecords;
	}

} 
```

*文件:StockDailyRecord.java*

```java
 package com.mkyong.stock;

import java.util.Date;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.GeneratedValue;
import static javax.persistence.GenerationType.IDENTITY;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.ManyToOne;
import javax.persistence.Table;
import javax.persistence.Temporal;
import javax.persistence.TemporalType;
import javax.persistence.UniqueConstraint;

@Entity
@Table(name = "stock_daily_record", catalog = "mkyongdb", 
uniqueConstraints = @UniqueConstraint(columnNames = "DATE"))
public class StockDailyRecord implements java.io.Serializable {

	private Integer recordId;
	private Stock stock;
	private Float priceOpen;
	private Float priceClose;
	private Float priceChange;
	private Long volume;
	private Date date;

	public StockDailyRecord() {
	}

	public StockDailyRecord(Stock stock, Date date) {
		this.stock = stock;
		this.date = date;
	}

	public StockDailyRecord(Stock stock, Float priceOpen, Float priceClose,
			Float priceChange, Long volume, Date date) {
		this.stock = stock;
		this.priceOpen = priceOpen;
		this.priceClose = priceClose;
		this.priceChange = priceChange;
		this.volume = volume;
		this.date = date;
	}

	@Id
	@GeneratedValue(strategy = IDENTITY)
	@Column(name = "RECORD_ID", unique = true, nullable = false)
	public Integer getRecordId() {
		return this.recordId;
	}

	public void setRecordId(Integer recordId) {
		this.recordId = recordId;
	}

	@ManyToOne(fetch = FetchType.LAZY)
	@JoinColumn(name = "STOCK_ID", nullable = false)
	public Stock getStock() {
		return this.stock;
	}

	public void setStock(Stock stock) {
		this.stock = stock;
	}

	@Column(name = "PRICE_OPEN", precision = 6)
	public Float getPriceOpen() {
		return this.priceOpen;
	}

	public void setPriceOpen(Float priceOpen) {
		this.priceOpen = priceOpen;
	}

	@Column(name = "PRICE_CLOSE", precision = 6)
	public Float getPriceClose() {
		return this.priceClose;
	}

	public void setPriceClose(Float priceClose) {
		this.priceClose = priceClose;
	}

	@Column(name = "PRICE_CHANGE", precision = 6)
	public Float getPriceChange() {
		return this.priceChange;
	}

	public void setPriceChange(Float priceChange) {
		this.priceChange = priceChange;
	}

	@Column(name = "VOLUME")
	public Long getVolume() {
		return this.volume;
	}

	public void setVolume(Long volume) {
		this.volume = volume;
	}

	@Temporal(TemporalType.DATE)
	@Column(name = "DATE", unique = true, nullable = false, length = 10)
	public Date getDate() {
		return this.date;
	}

	public void setDate(Date date) {
		this.date = date;
	}

} 
```

## 3.休眠配置文件

将带注释的类`Stock.java`和`StockDailyRecord.java`放在`hibernate.cfg.xml`中，如下所示:

*文件:hibernate.cfg.xml*

```java
 <?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration>
<session-factory>
    <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
    <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/mkyongdb</property>
    <property name="hibernate.connection.username">root</property>
    <property name="hibernate.connection.password">password</property>
    <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
    <property name="show_sql">true</property>
    <mapping class="com.mkyong.stock.Stock" />
    <mapping class="com.mkyong.stock.StockDailyRecord" />
</session-factory>
</hibernate-configuration> 
```

## 4.运行它

运行它，Hibernate 将在 STOCK 表中插入一行，在 STOCK_DAILY_RECORD 表中插入一行。

*文件:App.java*

```java
 package com.mkyong;

import java.util.Date;

import org.hibernate.Session;

import com.mkyong.stock.Stock;
import com.mkyong.stock.StockDailyRecord;
import com.mkyong.util.HibernateUtil;

public class App {
	public static void main(String[] args) {

        System.out.println("Hibernate one to many (Annotation)");
	Session session = HibernateUtil.getSessionFactory().openSession();

	session.beginTransaction();

	Stock stock = new Stock();
        stock.setStockCode("7052");
        stock.setStockName("PADINI");
        session.save(stock);

        StockDailyRecord stockDailyRecords = new StockDailyRecord();
        stockDailyRecords.setPriceOpen(new Float("1.2"));
        stockDailyRecords.setPriceClose(new Float("1.1"));
        stockDailyRecords.setPriceChange(new Float("10.0"));
        stockDailyRecords.setVolume(3000000L);
        stockDailyRecords.setDate(new Date());

        stockDailyRecords.setStock(stock);        
        stock.getStockDailyRecords().add(stockDailyRecords);

        session.save(stockDailyRecords);

	session.getTransaction().commit();
	System.out.println("Done");
	}
} 
```

*输出*

```java
 Hibernate one to many (Annotation)
Hibernate: 
    insert 
    into
        mkyongdb.stock
        (STOCK_CODE, STOCK_NAME) 
    values
        (?, ?)
Hibernate: 
    insert 
    into
        mkyongdb.stock_daily_record
        (DATE, PRICE_CHANGE, PRICE_CLOSE, PRICE_OPEN, STOCK_ID, VOLUME) 
    values
        (?, ?, ?, ?, ?, ?)
Done 
```

Download it – [Hibernate-one-to-many-annotation.zip](http://web.archive.org/web/20210507115125/http://www.mkyong.com/wp-content/uploads/2011/04/Hibernate-one-to-many-annotation.zip) (9KB)

## 参考

1.  [Hibernate 文档–一对多关系](http://web.archive.org/web/20210507115125/http://docs.jboss.org/hibernate/core/3.6/reference/en-US/html/mapping.html#d0e7576)

Tags : [hibernate](http://web.archive.org/web/20210507115125/https://mkyong.com/tag/hibernate/) [one-to-many](http://web.archive.org/web/20210507115125/https://mkyong.com/tag/one-to-many/)<input type="hidden" id="mkyong-current-postId" value="8690">