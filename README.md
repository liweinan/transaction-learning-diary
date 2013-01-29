## PostgreSQL JDBC

	public class PGXAConnection extends PGPooledConnection implements XAConnection, XAResource {

		public int prepare(Xid xid) throws XAException {
		    stmt.executeUpdate("PREPARE TRANSACTION '" + s + "'");
		}

		public void rollback(Xid xid) throws XAException {
		    stmt.executeUpdate("ROLLBACK PREPARED '" + s + "'");
		}

		public void commit(Xid xid, boolean onePhase) throws XAException {
		    if (onePhase)
		        commitOnePhase(xid);
		    else
		        commitPrepared(xid);
		}

		private void commitPrepared(Xid xid) throws XAException {
		    stmt.executeUpdate("COMMIT PREPARED '" + s + "'");
		}
	}

---

## org.hibernate.service.jta.platform.internal.JBossAppServerJtaPlatform

    protected TransactionManager locateTransactionManager()
    protected UserTransaction locateUserTransaction()

---

## EntityManagerFactory and UserTransaction

    @PersistenceUnit(unitName = "primary")
    private EntityManagerFactory entityManagerFactory;

    @Inject
    private UserTransaction userTransaction;

In AS7, UserTransaction is org.jboss.tm.usertx.client.ServerVMClientUserTransaction. EntityManagerFactory is org.hibernate.ejb.EntityManagerFactoryImpl.

---

## org.hibernate.ejb.EntityManagerImpl

    EntityManager entityManager = entityManagerFactory.createEntityManager();
    userTransaction.begin();
    entityManager.joinTransaction();
    userTransaction.commit();

---

* UserTransaction
* TransactionManager
* Transaction
* XAResource

* XAConnection

XAConnection is controller by XAResource.

---

* Transaction.enlistResource(XAResource)

* Transaction.delistResource(XAResource)

* XAConnection.getXAResource()

Retrieves an XAResource object that the transaction manager will use to manage this XAConnection object's participation in a distributed transaction.

TransactionManager <-> XAResource <-> XAConnection

---

What's the relationship between TransactionManager, Transaction and UserTransaction?

Transaction and UserTransaction are similar. In a managed environment, it's preferred to use UserTransaction provided by application server. It's not a preferred way to use TransactionManager to getTransaction(). The TransactionManager interface is used by application server design usually.

TransactionManager can be used to control transaction:

	tm = (TransactionManager)ctx.lookup("javax.transaction.TransactionManager");                             
	tx = (UserTransaction)ctx.lookup("javax.transaction.UserTransaction");
	tx.begin();                                     
	...
	transaction = tm.suspend();
	doNestedTransaction();                   
	tm.resume(transaction);
	...
	tx.commit();             
	
---

Good article to read:

* http://blog.sina.com.cn/s/blog_661a3fce0100mshi.html
* http://blog.sina.com.cn/s/blog_661a3fce0100msjb.html
* http://blog.sina.com.cn/s/blog_661a3fce0100msjv.html

---


