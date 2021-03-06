#### ParallelOrganizationServiceProxy.Update() Method

The Update method accepts a list of entities representing the targets of each update request. A method overload accepts an instance of [OrganizationServiceProxyOptions](OrganizationServiceProxyOptions-Class.md) to control proxy channel behaviors.  Nothing is returned by either method.

##### Method Overloads

```c#
public void Update(IEnumerable<Entity> targets)
public void Update(IEnumerable<Entity> targets, OrganizationServiceProxyOptions options)
```

Each of the above methods also provides an optional exception handling parameter of type **Action<TRequest, FaultException<OrganizationServiceFault>>**.

##### Usage Examples

Each example represents a method of a sample class that contains the following property

```c#
OrganizationServiceManager Manager { get; set; }
```

1. +How to Update entities in parallel +
Demonstrates a parallelized submission of multiple update requests

```c#
public void ParallelUpdate(List<Entity> targets)
{
    try
    {
        this.Manager.ParallelProxy.Update(targets);
    }
    catch (AggregateException ae)
    {
        // Handle exceptions
    }
}
```

2. +How to use the optional exception handler delegate+
Demonstrates a parallelized submission of multiple update requests with an optional delegate exception handling function. The delegate is provided the original request and the fault exception encountered. It is executed on the calling thread after all parallel operations are complete.

```c#
public void ParallelUpdateWithExceptionHandler(List<Entity> targets)
{
    int errorCount = 0;

    try
    {
        this.Manager.ParallelProxy.Update(targets,
            (target, ex) =>
            {
                System.Diagnostics.Debug.WriteLine("Error encountered during update of entity with Id={0}: {1}", target.Id, ex.Detail.Message);
                errorCount++;
            });
    }
    catch (AggregateException ae)
    {
        // Handle exceptions
    }

    Console.WriteLine("{0} errors encountered during parallel update.", errorCount);
}
```