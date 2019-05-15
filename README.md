# AngularFirestoreDatabase

(Adding Data to Cloud Firestore)[https://cloud.google.com/firestore/docs/manage-data/add-data]

## 1 The FireStore Database:
FireStore is a NoSQL document-oriented database.
All data is stored in objects called documents.

## 2:Reading Data from FireStore:
we have two options, we can either get a collection of items (think
of it as a list), or we can get a specific document from the database (like an object).

To read an object from the database, all you need to do is to create a reference pointing to that
document, for example:
  ```
  constructor(private afs: AngularFirestore) {
   this.userDoc = afs.doc<any>('userProfile/we45tfgy8ij');
  }
  ```
  We’re pointing to the document with the ID of we45tfgy8ij inside the userProfile collection.
  
 #### If you want to fetch the entire user collection it would be something like:
 ``` 
 constructor(private fireStore: AngularFirestore) {
     this.userProfileCollection = fireStore.collection<any>('userProfile');
   }
   
   ```
   
 #### You can also query users based on specific properties, let’s say our users have a property called
teamAdmin and we want to fetch the profiles of all the users who are admins of a team.
```
constructor(private fireStore: AngularFirestore) {
    this.teamAdminCollection = fireStore.collection<any>('userProfile', ref =>
     ref.where('teamAdmin', '==', true));
} 

  ```
  
## 3: Adding data to FireStore

To push objects to the database we have two main options, if we know the ID we want to give to
the document, we can use something like this:
```
constructor(private fireStore: AngularFirestore) {
this.userDoc = fireStore.doc<any>('userProfile/we45tfgy8ij'); // userProfile is a collection
this.userDoc.set({
name: 'Manzoor Alam',
email: 'manzoor@gmail.com',
// Other info you want to add here
})
```

If we don’t care about the generated ID, we can just push the documents to the collection and let
Firebase autogenerate those IDs.
```
constructor(private fireStore: AngularFirestore) {
this.userProfileCollection = fireStore.collection<any>('userProfile');
this.userProfileCollection.push({
name: 'Manzoor Alam',
email: 'manzoor@gmail.com',
// Other info you want to add here
});
}
}
  
  ```
Keep in mind that this way the ID for the user document won’t be the same as the user’s uid,
Firebase will autogenerate an ID for that document.

## 4: Updating data from FireStore:
 it’s called .update(), if we want to change the user’s name, for example, we will do something
like this:
```
constructor(private fireStore: AngularFirestore) {
this.userDoc = fireStore.doc<any>('userProfile/we45tfgy8ij');
this.userDoc.update({
name: 'Manzoor Alam',
email: 'manzoor@gmail.com',
// Other info you want to add here
})
```

## 5: Remove data from FireStore
There are a few ways we can remove data:
• Removing a specific document (an object).
• Removing a property or field from a document.
• Removing an entire collection.
Let’s explore them in that order, for example, I deleted a user from the Auth part of Firebase, and
now I want to remove the user profile from the database:
```
constructor(private fireStore: AngularFirestore) {
fireStore.doc<any>('userProfile/we45tfgy8ij').delete();
}
```
Or let’s say we don’t want to delete our user, but the user wrote and said to please delete his age
because he didn’t want anyone to know how old he was, in that case, we would fetch the user and
then remove the date field:
```
constructor(private fireStore: AngularFirestore) {
fireStore.doc<any>('userProfile/we45tfgy8ij').update({
age: firebase.firestore.FieldValue.delete()
});
}
```
## 6: 



















  
