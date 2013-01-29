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

In AS7, UserTransaction is org.jboss.tm.usertx.client.ServerVMClientUserTransaction.
EntityManagerFactory is org.hibernate.ejb.EntityManagerFactoryImpl.

---

## org.hibernate.ejb.EntityManagerImpl

    EntityManager entityManager = entityManagerFactory.createEntityManager();
    userTransaction.begin();
    entityManager.joinTransaction();
    userTransaction.commit();

---




