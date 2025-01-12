# UnityExtensions
Simple UnityExtensions to speed up your development workflow

## Usage
Download this repository as a zip and extract all the files in your assets folder. Make sure to create a folder called Misc and in it another folder called UnityExtensions and extract it there.
You can also use a different project structur, just make sure to adjust the namespaces then. If you want to use assemblies(recommended), adjust the YourProjectName.Misc.UnityExtensions.asmdef file name and the content inside to your project name!
if you dont want to use assemblies, just delete that file.

## Examples
### SingletonMonoBehaviour:
```csharp
public class Manager : SingletonMonoBehaviour<Manager> {

    public void DoSomething() { ... }
}

// In a different class from where you want to call it
public void Start() {
    Manager.Instance.DoSomething();
}
```
### Gizmox drawing multiple transforms:
**Before**
```csharp
protected override void OnDrawGizmosSelected() {
    base.OnDrawGizmosSelected();
        
    Gizmos.DrawWireSphere(ceilingCheck.position, ceilingCheckRadius);
        
    Gizmos.color = Color.magenta;
    Gizmos.DrawWireSphere(topLadderCheck.position, climbCheckRadius);
    Gizmos.DrawWireSphere(middleLadderCheck.position, climbCheckRadius);
    Gizmos.DrawWireSphere(feetLadderCheck.position, climbCheckRadius);
    if (isFacingRight) {
        Gizmos.DrawWireSphere(wallCheck.position, edgeClimbCheckRadius);
        Gizmos.DrawWireSphere(edgeCheck.position, edgeClimbCheckRadius);
    } else {
        Vector2 invertedWallCheckPosition = MirrorChildPosition(wallCheck);
        Vector2 invertedEdgeCheckPosition = MirrorChildPosition(edgeCheck);
            
        Gizmos.DrawWireSphere(invertedWallCheckPosition, edgeClimbCheckRadius);
        Gizmos.DrawWireSphere(invertedEdgeCheckPosition, edgeClimbCheckRadius);
    }
}
```

**After**
```csharp
protected override void OnDrawGizmosSelected() {
    base.OnDrawGizmosSelected();
        
    Gizmox.WireSpheres(ceilingCheckRadius)
        .DrawFromTransform(ceilingCheck)
        .Radius(climbCheckRadius)
        .Color(Color.magenta)
        .DrawFromTransform(topLadderCheck, middleLadderCheck, feetLadderCheck)
        .When(isFacingRight)
        .DrawFromTransform(wallCheck, edgeCheck)
        .Else()
        .DrawFromVector(MirrorChildPosition(wallCheck), MirrorChildPosition(edgeCheck));
}
```
**You can also directly parse arrays of the transforms or vectors:**
```csharp
Transform[] objects = {object1, object2};
Gizmox.WireSpheres(radius).DrawFromTransform(objects);
```

### Gizmox drawing text:
**Drawing a single string:**
```csharp
Gizmos.color = Color.cyan;
Gizmox.DrawText(text, position);
```

**Drawing multiple common strings:**
```csharp
Gizmox.Text("Common text between objects").DrawFromTransform(object1, object2, object3);
```

**Drawing multiple unique strings:**
```csharp
Gizmox.Text("Common text between objects").DrawTextFromTransform(new("Im steve", object1), new("Im a car", object2));
```
