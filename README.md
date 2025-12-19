# lessons-learned-the-hard-way

## HashCode and Equals on Sets
### Story
Mirgrating
```java
    @ToString.Exclude
    @ElementCollection(fetch = FetchType.EAGER)
    Set<String> allowedClientIds;
```
to
```
    @ToString.Exclude
    @ElementCollection(fetch = FetchType.EAGER)
    @CollectionTable(name = "MY_ALLOWED_CLIENT_IDS")
    @Builder.Default
    Set<Client> allowedClientIds = new HashSet<>();
```
with
```java
@Embeddable
public class Client {

    @Column(name = "allowed_client_ids")
    String id;

}
```

## Error Symptoms
Dozens of `org.springframework.orm.ObjectOptimisticLockingFailureException: Row was updated or deleted by another transaction (or unsaved-value mapping was incorrect)`

### Solution
Implement equals and hashCode on the Client class. Clients are held in a Set of the owning entity, therefore proper equals and hashCode implementations are needed.
