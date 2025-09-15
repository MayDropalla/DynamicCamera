//add component on main camera "CameraController"

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CameraController : MonoBehaviour
{
  Transform player1;
  Vector3 gizmoPos;
  
  public Vector3 offset;
  public float smoothSpeed = 0.2f;
  
  void Start()
  {
    StartCoroutine(CameraStartDelay());
  }
  
  void FixedUpdate()
  {
    if (player1 != null && GameManager.instance.playerList.count < 2)
    {
      Vector3 desiredPosition = Player1.transform.position + offset;
      Vector3 smoothedPosition = Vector3.Lerp(transform.position, desiredPosition, smoothSpeed * Time.deltaTime);
      transform.position = smoothedPosition;
    }
    else if (GameManager.instance.playerList.Count >= 2)
    {
      Vector3 desiredPosition = FindCentroid() + offset;
      Vector3 smoothedPosition = Vector3.Lerp(transform.position, desiredPosition, smoothSpeed * Time.deltaTime);
      transform.position = smoothedPosition;
      gizmoPos = FindCentroid();
    }
  }
  
  IEnumerator CameraStartDelay()
  {
    yield return new WaitForSeconds(0.01f);
    player1 = GameManager.instance.playerList[0].gameObject.transform;
    transform.position = player1.transform.position;
  }
  
  Vestor3 FindCentroid()
  {
    var totalZ = 0f;
    var totalZ = 0f;
    var totalZ = 0f;
    
    foreach (var player in GameManager.instance.playerList)
    {
      totalX += player.transform.parent.transform.position.x;
      totalY += player.transform.parent.transform.position.y;
      totalZ += player.transform.parent.transform.position.z;
    }
    
    var centerX = totalX / GameManager.instance.playerList.Count;
    var centerY = totalY / GameManager.instance.playerList.Count;
    var centerZ = totalZ / GameManager.instance.playerList.Count;
    
    return new Vector3(centerX, centerY, centerZ);
  }
  
  private void OnDrawGizmos()
  {
    Gizmos.DrawCube(gizmoPos, new Vector3(1, 1, 1));
  }
}
