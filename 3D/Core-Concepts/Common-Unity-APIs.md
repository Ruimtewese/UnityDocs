# Common Unity APIs: Quick Reference

## GameObject and components

    GetComponent<Rigidbody>()
    GetComponentInChildren<Collider>()
    TryGetComponent(out Rigidbody body)
    gameObject.SetActive(false)
    GetComponent<Renderer>().enabled = false

Cache references in Awake when used repeatedly. GetComponent searches the requested object; use parent/child lookup when hierarchy placement requires it. Avoid scene-wide searches in Update.

## Instantiate and Destroy

    GameObject clone = Instantiate(prefab, position, rotation);
    Destroy(clone, 2f);

Instantiate creates a runtime object. Destroy is commonly used for one-off objects; frequent effects may be better pooled. A prefab asset cannot reliably store a direct reference to an arbitrary scene instance.

## Transform

    transform.position
    transform.localPosition
    transform.rotation
    transform.localRotation
    transform.forward
    transform.right
    transform.up
    transform.TransformPoint(localPoint)
    transform.InverseTransformPoint(worldPoint)

Position is world space; localPosition is relative to the parent. TransformPoint converts a local point to world space; InverseTransformPoint does the opposite. forward/right/up represent the object's world orientation axes.

## Time and random

    Time.deltaTime
    Time.fixedDeltaTime
    Time.timeScale
    Random.Range(0f, 1f)
    Random.Range(0, count)

For integer Random.Range, the maximum is exclusive. For float Random.Range, both ends can be inclusive. Validate count before requesting a random index.

## Math and interpolation

    Mathf.Clamp(value, min, max)
    Mathf.Clamp01(value)
    Mathf.Lerp(a, b, t)
    Mathf.MoveTowards(current, target, maxDelta)
    Mathf.Approximately(a, b)
    Vector3.Distance(a, b)
    Vector3.Dot(a, b)
    Vector3.Cross(a, b)
    Vector3.Normalize(direction)
    Quaternion.LookRotation(direction)

Lerp interpolates by a fraction; repeated Lerp is not constant-speed motion. MoveTowards limits the maximum step distance.

## Scene and diagnostics

    SceneManager.LoadScene("Level01");
    Debug.Log("Message", this);
    Debug.DrawRay(origin, direction * range, Color.yellow, 1f);

Scene loading requires inclusion in the active Build Profile.

Official reference: https://docs.unity3d.com/ScriptReference/
