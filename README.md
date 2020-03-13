# Naming Convention

## Custom Applications
| Label | Api Name |
|---|---|
| Application | Application |
| My Application | MyApplication |

## Custom Objects
| Label | Api Name |
|---|---|
| Product | Product__c |
| Product Class | ProductClass__c |

## Custom Fields
| Label | Api Name |
|---|---|
| Code | Code__c |
| Credit Card | CreditCard__c |

## Visualforce pages that overwrites standard buttons and links
| Standard button/link | Page name |
|---|---|
| - | PageName |
| Tab | ObjectNameTab |
| List | ObjectNameList |
| View | ObjectNameView |
| Edit | ObjectNameEdit |
| Delete | ObjectNameDelete |
| Clone | ObjectNameClone |
| New | ObjectNameNew |
| Accept | ObjectNameAccept |

## Apex Classes
| Used as | Class Name |
|---|---|
| Visualforce extension | PageNameExt |
| Visualforce controller | PageNameCtrl |
| Lightning Component controller | ComponentCtrl |
| TriggerHandler | ObjectNameTriggerHandler |
| Utils class | ClassNameUtils |
| API Rest | ClassNameRest |
| Batchable | ClassNameBatch |
| Schedulable | ClassNameSched |
| Test | ClassNameTest (i.e. ClassNameBatchTest) |

## Apex Triggers
| Object | Trigger Name |
|---|---|
| Account | AccountTrigger |
| Custom Object | CustomObjectTrigger |

## Apex Code
| Element | Convention | Example |
|---|---|---|
| Class Name | Class names should be nouns in UpperCamelCase | public class ClassName |
| Method Name | Methods should be verbs and in loverCamelCase | public void doSomething() { } |
| Property/Variable Name| Variable name should be in lowerCamelCase | public String myName |
| Constant| Constant name should be written in upper characters separated by underscores | static final Integer MY_INT |

# Best Practices

## Trigger

##### AccountTrigger.trigger
```java
trigger  AccountTrigger  on  Account (before  insert,  after  insert,  before  update,  after  update,  before  delete,  after  delete) {
	
	if(AccountTriggerHandler.runOnce() &&  !AccountTriggerHandler.manualSkip) {
		
		/* Before Insert */
		if(Trigger.isInsert && Trigger.isBefore) {
			AccountTriggerHandler.foo(Trigger.new);
		}
		
		/* After Insert */
		else  if(Trigger.isInsert && Trigger.isAfter) {
		}
		
		/* Before Update */
		else  if(Trigger.isUpdate && Trigger.isBefore) {
		}
		
		/* After Update */
		else  if(Trigger.isUpdate && Trigger.isAfter) {
		}
		
		/* Before Delete */
		else  if(Trigger.isDelete && Trigger.isBefore) {
		}
		
		/* After Delete */
		else  if(Trigger.isDelete && Trigger.isAfter) {
		}
	}
```

##### AccountTriggerHandler.cls
```java
public without sharing class  AccountTriggerHandler {

	public  static  Boolean manualSkip =  false;

	private  static  boolean run =  true;
	public  static  boolean  runOnce(){
		if(run) {
			run  =  false;
			return  true;
		} else {
			return  run;
		}
	}
	
	public  static  String  getRecordTypeDeveloperName(Id  recordTypeId) {
		Map<Id,Schema.RecordTypeInfo> rtMap =  Account.sobjectType.getDescribe().getRecordTypeInfosById();
		return  rtMap.get(recordTypeId).getDeveloperName();
	}
	
	public  static  void  foo(List<Account> triggerNew) {
		for(Account c :  triggerNew) {
			switch  on  getRecordTypeDeveloperName(c.RecordTypeId) {
				when  'Pippo' {
					System.debug(c);
				}
				when  'Pluto' {
					System.debug(c);
				}
				when  else {
					System.debug(c);
				}
			}
		}
	}
}
```