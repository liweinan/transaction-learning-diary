## PostgreSQL JDBC

   

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



