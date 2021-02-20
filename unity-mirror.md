[back to overwiev](/../..)

# Unity Mirror CheatSheet

## Table of Contents

- [NetworkManager](#networkmanager)
- [SyncVars](#syncvars)

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
