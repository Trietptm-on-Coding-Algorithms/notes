When we’re performing various database interactions in the context of a business operation, we’ll likely want to maintain atomicity in the event of one of the interactions failing. The typical example is a bank account transfer. We’ll credit one account then debit the other. If something goes wrong, we want to rollback. Otherwise we’d be left in an inconsistent state.

Database transactions are the typical solution to this class of problem. We’ll like to start a transaction and perform some unit of work before finally committing. If a problem occurs during the execution, we should rollback. We don’t want to do this ad-hoc with various catch statements. If we did, it would be hard to manage and to be sure we’ve got all the cases. We could even ‘double up’ and handle exceptions twice.

In the example below we’ve chosen to handle the exception by rolling back the transaction and interestingly, rethrowing the exception. Although we’ve identified this database interaction as a boundary, by rethrowing the exception, we’re recognising that there are additional boundaries to consider. In the context of a database call, for example, the exception could propagate up to the UI. We’ve handled the exception here to maintain data integrity and allowed other exception handling policies to be applied. It’s a good example of an internal boundary.
```
public <T, E extends Exception> T run(UnitOfWork<T, E> unitOfWork) throws Throwable {
    Session session = sessionProvider.getCurrentSession();
    Transaction transaction = session.beginTransaction();
    try {
        T result = unitOfWork.execute(sessionProvider);
        transaction.commit();
        return result;
    } catch (Throwable e) {
        transaction.rollback();
        throw e;
    } finally {
        if (session.isOpen())
            session.close();
    }
}
```
