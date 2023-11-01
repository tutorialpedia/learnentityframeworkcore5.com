# EFCore BulkExtensions

In EF Core, if you want to improve your CRUD performance, you need to call [BulkExtensions](https://entityframework-extensions.net/bulk-extensions) from the library made by [ZZZ Projects](https://zzzprojects.com/).

They provide all common bulk extensions required when working with EF Core:

- [Bulk SaveChanges](https://entityframework-extensions.net/bulk-savechanges)
- [Bulk Insert](https://entityframework-extensions.net/bulk-insert)
- [Bulk Update](https://entityframework-extensions.net/bulk-update)
- [Bulk Delete](https://entityframework-extensions.net/bulk-delete)
- [Bulk Merge](https://entityframework-extensions.net/bulk-merge)
- [Bulk Synchronize](https://entityframework-extensions.net/bulk-synchronize)
- [Bulk Read](https://entityframework-extensions.net/bulk-read)
- [Where Bulk Contains](https://entityframework-extensions.net/where-bulk-contains)
- [Where Bulk Not Contains](https://entityframework-extensions.net/where-bulk-not-contains)

And even more features through their free library [Entity Framework Plus](https://entityframework-plus.net/).

## How to create a Bulk Operations?

Without options, it's pretty simple. Instead of tracking your entities, you simply pass it to one of their methods, such as Bulk Insert:

```csharp
context.BulkInsert(customers);
```

If you need an option like including all your entities graph, you need to create a lambda expression such as:

```csharp
context.BulkInsert(customers, options => { options.IncludeGraph = true});
```

## Why using Bulk Operations in EF Core?

The main reason people need to use Bulk Operations in EF Core is to improve their performance when importing thousand of entities. In addition to saving data, you also reduce your memory usage.

When processing a lot of entities, using Bulk Extensions instead of `SaveChanges` can be 5 times faster and use 20% of the memory.

## References

- [EFCore BulkExtensions](https://entityframework-extensions.net/bulk-extensions)
- [EF Core BulkExtensions Download](https://www.nuget.org/packages/Z.EntityFramework.Extensions.EFCore/)
