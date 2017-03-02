
/// Prefab replacement - attach this script to a gameobject in the scene.
/// Identifies all child prefabs in a selected parent prefab at runtime. Then allows you to delete them and replace them with 
/// the relevant with updated versions
/// script by sketchyGio/inkGio (curse the guy who stole my screenname on github)


//	Instructions:
//	1. Add script to a gameobject in the scene. Also make sure to place the PrefabReplaceEditor script inside the Editor folder
//	2. Assign the target parent prefab through the inspector.
//	3. Play the scene.
//	4. All child gameobjects are populated added under the ChildrenPrefabs section in the inspector.
//	5. All UNIQUE names of prefabs are added once under NameOfOldPrefabs.
//	6. While the scene is playing, select the updated replacement prefabs in the ReplacementPrefabs inspector field. 
//	   These should mirror the NameOfOldPrefabs
//	   If you don't want to replace a certain prefab, you can leave the appropriate field blank.
//	7. Check the bool box "Replace Instances" in the inspector and the script should instantly delete old children 
//	   prefabs and replace them with the new ones.


using UnityEngine;
using System.Collections;
using System.Collections.Generic;

//Prefab Replacement script
public class PrefabReplacement : MonoBehaviour
{
	[HideInInspector] public GameObject parentPrefab;

	//List to dynamically hold each unique type of instance
	List<UniqueInstance> uniqueInstanceList = new  List<UniqueInstance>();

	bool renameChildrenInstances = false;
	//Array will hold every actual child instance inside parent object
	public GameObject[] childrenPrefabs;
	int uniquePrefabNum = 0;

	//These two arrays will be resized according to how many unique instances are added to uniqueInstanceList
	//At runtime, you will need to allocate replacement prefabs for replacementPrefabs[], to match the names of the
	//instances you want replaced from nameOfOldPrefabs[]
	public string[] nameOfOldPrefabs;
	public GameObject[] replacementPrefabs;

	//This bool will be checked by you in the inspector to activate the replacement process
	bool replaceInstances = false;

	bool childrenRenamed = false;

	public void RenameInstances()
	{
		replaceInstances = false;
		uniqueInstanceList.Clear();
		uniqueInstanceList = new  List<UniqueInstance>();
		uniquePrefabNum = 0;
		nameOfOldPrefabs = new string[0];
		replacementPrefabs = new GameObject[0];

		if(parentPrefab == null)
			return;

		//Set length of array based on number of children in gameobject
		childrenPrefabs = new GameObject[parentPrefab.transform.childCount];

		//Assign every instance in the parent object to an element in the array
		for(int i = 0; i < childrenPrefabs.Length; i++)
		{
			childrenPrefabs[i] = parentPrefab.transform.GetChild(i).gameObject;
		}



		//Iterate through every instance. If it's the first time this instance is being seen, save it in a list
		bool duplicatePrefabFound = false;

		string nameOfInstance;

		for(int i = 0; i < childrenPrefabs.Length; i++)
		{
			nameOfInstance = childrenPrefabs[i].name;
			//print(childrenPrefabs[i].name);
			for(int j = 0; j  < uniquePrefabNum; j++)
			{
				//Find the end of the last enclosing parentheses from names likes object(Clone)(123)
				char[] charName = nameOfInstance.ToCharArray();
				if(charName[charName.Length - 1] == ')')
				{
					string parenthesesContents = null;
					int parenthesesStart = 0;
					int parenthesesEnd = charName.Length;

					//Find the beginning of the last enclosing parentheses
					for(int k = charName.Length - 1; k >= 0; k--)
					{
						if(charName[k] == '(')
						{
							parenthesesStart =  k;
							break;
						}
					}

					//Convert parentheses contents into a number to see if they're using the deafult unity 
					//cloning format If the parentheses do enclose numbers, delete those numbers, so all 
					// clones of that type end up having the same name
					if(parenthesesStart != 0)
					{
						for(int l = parenthesesStart + 1; l < parenthesesEnd - 1; l++)
						{
							parenthesesContents = parenthesesContents + charName[l].ToString();
						}

						int parenthesesNumber;
						if(int.TryParse(parenthesesContents,out parenthesesNumber))
						{
							print("Removed (" + parenthesesContents + ") from name " + nameOfInstance);
							nameOfInstance = nameOfInstance.Substring(0, nameOfInstance.Length - (nameOfInstance.Length - parenthesesStart + 1));
							childrenPrefabs[i].name = nameOfInstance;
						}
					}
				}

				//Do not add duplicate item names to unique instance array
				if(nameOfInstance == uniqueInstanceList[j].name)
					duplicatePrefabFound = true;
			}

			//If no duplicates are found, increase the total number of unique prefabs, and set bool to 
			//false for next loop
			if(duplicatePrefabFound == false)
			{
				uniquePrefabNum++;
				uniqueInstanceList.Add( new UniqueInstance(nameOfInstance) );
				print(nameOfInstance + " found as child instance type # " + i);
			}
			duplicatePrefabFound = false;
		}

		nameOfOldPrefabs = new string[uniqueInstanceList.Count];
		replacementPrefabs = new GameObject[uniqueInstanceList.Count];

		for(int i = 0; i < nameOfOldPrefabs.Length; i++)
		{
			nameOfOldPrefabs[i] = uniqueInstanceList[i].name;
		}
		renameChildrenInstances = false;
		childrenRenamed = true;
	}



	//Once the script runs, inspector should create a new field for each unique prefab, under NamesOfOldPrefabs
	//These fields are meant as a reference for the user, in order to use the PrefabReplacement fields
	//User should assign updated prefabs through the PrefabReplacement fields at runtime, matching the outdated prefabs 
	//mentioned in NamesOfOldPrefabs
	public void ReplaceInstances()
	{
		//Once the updated prefabs are assigned, set bool to true in inspector to manually activate the search and replace
		if(parentPrefab != null)
		{
			//For each selected prefab object in the inspector (that is not null), find and replace every selected item from 
			//the childrenPrefabs[] array

			for(int i=0; i < replacementPrefabs.Length; i++)
			{
				if(replacementPrefabs[i] != null)
				{
					for(int j=0; j < childrenPrefabs.Length; j++)
					{

						if(childrenPrefabs[j] != null && childrenPrefabs[j].name == nameOfOldPrefabs[i]) 
						{
							Vector3 instancePos = childrenPrefabs[j].transform.position;
							DestroyImmediate(childrenPrefabs[j]);
							GameObject newInstance;
							Instantiate(replacementPrefabs[i],instancePos, Quaternion.identity, parentPrefab.transform);
						}
					}
				}
			}
			replaceInstances = false;
			print("Finished replacing instances with updated prefab clones. Save the current parent prefab instance before exiting the scene preview!");
		}
	}
		
}

public class UniqueInstance : MonoBehaviour 
{
	public string name;

	public UniqueInstance(string instanceName)
	{
		name = instanceName;
	}
}
