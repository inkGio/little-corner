//Place this script inside your Assets/Editor folder. If you do not have an Editor folder, make one inside the Assets directory

using UnityEngine;
using System.Collections;
using UnityEditor;

[CustomEditor(typeof(PrefabReplacement))]
public class PrefabReplacerEditor : Editor 
{
	GameObject lastParentPrefab = null;
	bool showReplaceButton = false;
	int number;
	public override void OnInspectorGUI()
	{			

		PrefabReplacement prefabReplacement = (PrefabReplacement)target;
		//GameObject parentPrefabProp = serializedObject.FindProperty("parentPrefab");

		//Field to insert parent prefab
		prefabReplacement.parentPrefab = (GameObject)EditorGUILayout.ObjectField ("Parent Prefab", prefabReplacement.parentPrefab, typeof(GameObject), true);

		//Checks to see whether the current prefab matches the one from the previous frame. If it does not,
		//hides the Replace button so user can't apply incorrect settings from previous prefab to the new one
		if(lastParentPrefab == null || prefabReplacement.parentPrefab != lastParentPrefab)
		{
			showReplaceButton = false;

			if(prefabReplacement.parentPrefab != null)
				lastParentPrefab = prefabReplacement.parentPrefab;
		}

		EditorGUILayout.IntField("number", number);

		//First Button only appears if a parent prefab was selected
		if(prefabReplacement.parentPrefab != null && GUILayout.Button("Rename children instances"))
		{
			number++;
			prefabReplacement.RenameInstances();

			if(lastParentPrefab == prefabReplacement.parentPrefab)
				showReplaceButton = true;
		}


		DrawDefaultInspector();


		if(showReplaceButton)
		{
			if(GUILayout.Button("Replace children instances"))
			{
				number++;

				prefabReplacement.ReplaceInstances();
				showReplaceButton = true;

			}
		}
	}

}
