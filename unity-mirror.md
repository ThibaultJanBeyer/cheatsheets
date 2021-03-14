[back to overwiev](/../..)

# Unity Mirror CheatSheet

## Table of Contents

- [NetworkManager](#networkmanager)
- [NetworkBehaviour](#networkbehaviour)
- [SyncVars](#syncvars)
- [Actions](#syncvars)
- [Server Authority](#server-authority)
- [Network Transform](#network-transform)
- [Spawning](#spawning)
- [Useful](#useful)

## NetworkManager
Example:
```c#
public class MyNetworkManager : NetworkManager
{
    public override void OnClientConnect(NetworkConnection conn) // called on the client when it's connected to the server
    {
        base.OnClientConnect(conn);
        Debug.Log("I have connected to a server!");
    }

    public override void OnServerAddPlayer(NetworkConnection conn) // called on the server whenever a player object is added after a client connected
    {
        base.OnServerAddPlayer(conn);
        MyNetworkPlayer player = conn.identity.GetComponent<MyNetworkPlayer>(); // conn.identity is a reference to the player object that was created
        player.SetDisplayName($"Player {this.numPlayers}"); // i.e. run a script on the player in
        Debug.Log($"Connected: {this.numPlayers} Players"); // numPlayers holds a count of the amount of players
    }
}
```

## NetworkBehaviour
Example:
```c#
public class MyNetworkPlayer : NetworkBehaviour
{
    public override void OnStartAuthority() // called only on the client that owns the object as soon as authority was given by the server
    {
        base.OnStartAuthority();
    }
    
    [ClientCallback] // makes sure that it's not run on the server
    private void Update()
    {
        if(!this.hasAuthority) return; // checks if the current client is the owner of the object
    }
}
```

## SyncVars

Example:
```c#
public class MyNetworkPlayer : NetworkBehaviour
{
    [SyncVar] // syncs a variable from the server accross all clients
    [SerializeField] // show the variable in the editor
    private string displayName = "Missing Name";

    [Server] // only runs code on the server
    public void SetDisplayName(string newDisplayName)
    {
        Debug.Log(newDisplayName);
        displayName = newDisplayName;
    }
}
```

With hooks
```c#
    [SyncVar(hook=nameof(HandleDisplayNameUpdated))] // hook will call the specified method on the client every time the var is updated
    private string displayName = "Missing Name";

    private void HandleDisplayNameUpdated(string oldDisplayName, string newDisplayName)
    {
        Debug.Log(newDisplayName);
    }
```

## Actions

Commands (clients calling a method on the server):
```c#
    [Command]
    private void CmdSetDisplayName(string newDisplayName) // this will only be executed on the server
    {
        SetDisplayName(newDisplayName);
    }
    
    [ContextMenu("SetMyName")] // this is not nescessary, it's to make the method available in the unity editor
    private void SetMyName()
    {
        CmdSetDisplayName("My New Name"); // this is executed on the client, it will tell the server to execute the command method on the server
    }
    
```

CleintRpc (server calling a method on all clients):
```c#
    [Command]
    private void CmdSetDisplayName(string newDisplayName) // executed on the server
    {
        SetDisplayName(newDisplayName);
        RpcLogName(newDisplayName); // method call
    }

    [ClientRpc]
    private void RpcLogName(string name) // this method will be called on all clients
    {
        Debug.Log($"The new name is: {name}");
    }
```

TargetRpc (server calling a method on a specific client (only owner if nothing is specified)):
```c#
tbd
```

## Server Authority

- SyncVars have authority by default, only the server can change them and have them synced
- Spawned player also has ownership authority by default. When spawning other objects it's important to tell Mirror who owns what.
- The server should check the validity of each client command:
```c#
    [Command]
    private void CmdSetDisplayName(string newDisplayName) // executed on the server
    {
        if(newDisplayName.Length < 2) return;
        SetDisplayName(newDisplayName);
        RpcLogName(newDisplayName); // method call
    }
```

## Network Transform

- Use a component called `NetworkTransform` if you want to sync the transform attributes of that object across all clients.
- It will automatically transpolate smoothly between the positions.

## Spawning

```c#
// IPointerClickHandler is an interface. You can use it to have something happening when the user clicks the object it is attached to
public class UnitSpawner : NetworkBehaviour, IPointerClickHandler
{
    [SerializeField] private GameObject unitPrefab = null;
    [SerializeField] private Transform unitSpawnPoint = null;


    #region Server

    [Command]
    private void CmdSpawnUnit()
    {
        GameObject unitInstance = Instantiate(unitPrefab, unitSpawnPoint.position, unitSpawnPoint.rotation);
        // Spawn takes 2 arguments: 1st: the object instance, 2nd: the owner of the object (if empty, the server will be the owner)
        // connectionToClient is the connection that owns the object that called the command
        NetworkServer.Spawn(unitInstance, connectionToClient);
    }

    #endregion

    #region Client

    // this comes from IPointerClickHandler Interface
    public void OnPointerClick(PointerEventData eventData)
    {
        if(eventData.button != PointerEventData.InputButton.Left || !hasAuthority) return;
        CmdSpawnUnit();
    }

    #endregion
}
```

## Useful

- You can create sections in a class by using `#region RegionName` and `#endregion`
- You can have a method being call-able from the unity editor by adding the `[ContextMenu("Name")]` decorator
- Template strings in c# are `$"Something {variable}"`
