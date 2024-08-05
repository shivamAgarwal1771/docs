#set( $PK = "c#${ctx.args.input.CallId}" )
#set( $UpdatedAt = $util.defaultIfNull($ctx.args.input.UpdatedAt, $util.time.nowISO8601() ) )
$util.qr( $ctx.args.input.put("UpdatedAt", $UpdatedAt ) )
## Set up some space to keep track of things we're updating **
#set( $expNames = {} )
#set( $expValues = {} )
#set( $expSet = {} )
## Iterate through each argument with values and update expression variables **
#foreach( $entry in $ctx.args.input.entrySet() )
    ## avoid overwriting CreatedAt and skip empty values **
    #if( $entry != "CreatedAt" && !$util.isNullOrBlank($entry.value)  )
        $util.qr( $expSet.put("#${entry.key}", ":${entry.key}") )
        $util.qr( $expNames.put("#${entry.key}", "${entry.key}") )
        $util.qr( $expValues.put(":${entry.key}", $util.dynamodb.toDynamoDB($entry.value)) )
    #end
#end
## Start building the update expression, starting with attributes we're going to SET **
#set( $expression = "" )
#if( !${expSet.isEmpty()} )
    #set( $expression = "SET" )
    #foreach( $entry in $expSet.entrySet() )
        #if( $entry.key == "#CallCategories" )
            $util.qr( $expValues.put(":EmptyList", $util.dynamodb.toDynamoDB([])) )
            #set( $expression = "${expression} ${entry.key} = list_append(if_not_exists(${entry.key}, :EmptyList), ${entry.value})" )
        #else
            #set( $expression = "${expression} ${entry.key} = ${entry.value}" )
        #end
        #if ( $foreach.hasNext )
            #set( $expression = "${expression}," )
        #end
    #end
#end
{
  "version" : "2018-05-29",
  "operation" : "UpdateItem",
  "key": {
    "PK": $util.dynamodb.toDynamoDBJson($PK),
    "SK": $util.dynamodb.toDynamoDBJson($PK),
  },
  "update" : {
    "expression": "$expression"
    #if( !${expNames.isEmpty()} )
      , "expressionNames": $utils.toJson($expNames)
    #end
    #if( !${expValues.isEmpty()} )
      , "expressionValues": $utils.toJson($expValues)
    #end
  },
  "condition": {
    ## Only update if item already exists and UpdatedAt is newer **
    "expression": "attribute_exists(#PK) AND #UpdatedAt < :UpdatedAt",
    "expressionNames": {
      "#PK": "PK",
      "#UpdatedAt": "UpdatedAt"
    },
    "expressionValues": {
      ":UpdatedAt": $utils.dynamodb.toDynamoDBJson($UpdatedAt)
    }
  }
}


this is update agent mutation 

#set( $PK = "c#${ctx.args.input.CallId}" )
#set( $UpdatedAt = $util.defaultIfNull($ctx.args.input.UpdatedAt, $util.time.nowISO8601() ) )
$util.qr( $ctx.args.input.put("UpdatedAt", $UpdatedAt ) )
## Set up some space to keep track of things we're updating **
#set( $expNames = {} )
#set( $expValues = {} )
#set( $expSet = {} )
## Iterate through each argument with values and update expression variables **
#foreach( $entry in $ctx.args.input.entrySet() )
    ## avoid overwriting CreatedAt and skip empty values **
    #if( $entry != "CreatedAt" && !$util.isNullOrBlank($entry.value)  )
        $util.qr( $expSet.put("#${entry.key}", ":${entry.key}") )
        $util.qr( $expNames.put("#${entry.key}", "${entry.key}") )
        $util.qr( $expValues.put(":${entry.key}", $util.dynamodb.toDynamoDB($entry.value)) )
    #end
#end
## Start building the update expression, starting with attributes we're going to SET **
#set( $expression = "" )
#if( !${expSet.isEmpty()} )
    #set( $expression = "SET" )
    #foreach( $entry in $expSet.entrySet() )
        #if( $entry.key == "#CallCategories" )
            $util.qr( $expValues.put(":EmptyList", $util.dynamodb.toDynamoDB([])) )
            #set( $expression = "${expression} ${entry.key} = list_append(if_not_exists(${entry.key}, :EmptyList), ${entry.value})" )
        #else
            #set( $expression = "${expression} ${entry.key} = ${entry.value}" )
        #end
        #if ( $foreach.hasNext )
            #set( $expression = "${expression}," )
        #end
    #end
#end
{
  "version" : "2018-05-29",
  "operation" : "UpdateItem",
  "key": {
    "PK": $util.dynamodb.toDynamoDBJson($PK),
    "SK": $util.dynamodb.toDynamoDBJson($PK),
  },
  "update" : {
    "expression": "$expression"
    #if( !${expNames.isEmpty()} )
      , "expressionNames": $utils.toJson($expNames)
    #end
    #if( !${expValues.isEmpty()} )
      , "expressionValues": $utils.toJson($expValues)
    #end
  },
  "condition": {
    ## Only update if item already exists and UpdatedAt is newer **
    "expression": "attribute_exists(#PK) AND #UpdatedAt < :UpdatedAt",
    "expressionNames": {
      "#PK": "PK",
      "#UpdatedAt": "UpdatedAt"
    },
    "expressionValues": {
      ":UpdatedAt": $utils.dynamodb.toDynamoDBJson($UpdatedAt)
    }
  }
}

this is the mutation for issue detected

#set( $PK = "c#${ctx.args.input.CallId}" )
#set( $UpdatedAt = $util.defaultIfNull($ctx.args.input.UpdatedAt, $util.time.nowISO8601() ) )
$util.qr( $ctx.args.input.put("UpdatedAt", $UpdatedAt ) )
## Set up some space to keep track of things we're updating **
#set( $expNames = {} )
#set( $expValues = {} )
#set( $expSet = {} )
## Iterate through each argument with values and update expression variables **
#foreach( $entry in $ctx.args.input.entrySet() )
    ## avoid overwriting CreatedAt and skip empty values **
    #if( $entry != "CreatedAt" && !$util.isNullOrBlank($entry.value)  )
        $util.qr( $expSet.put("#${entry.key}", ":${entry.key}") )
        $util.qr( $expNames.put("#${entry.key}", "${entry.key}") )
        $util.qr( $expValues.put(":${entry.key}", $util.dynamodb.toDynamoDB($entry.value)) )
    #end
#end
## Start building the update expression, starting with attributes we're going to SET **
#set( $expression = "" )
#if( !${expSet.isEmpty()} )
    #set( $expression = "SET" )
    #foreach( $entry in $expSet.entrySet() )
        #if( $entry.key == "#CallCategories" )
            $util.qr( $expValues.put(":EmptyList", $util.dynamodb.toDynamoDB([])) )
            #set( $expression = "${expression} ${entry.key} = list_append(if_not_exists(${entry.key}, :EmptyList), ${entry.value})" )
        #else
            #set( $expression = "${expression} ${entry.key} = ${entry.value}" )
        #end
        #if ( $foreach.hasNext )
            #set( $expression = "${expression}," )
        #end
    #end
#end
{
  "version" : "2018-05-29",
  "operation" : "UpdateItem",
  "key": {
    "PK": $util.dynamodb.toDynamoDBJson($PK),
    "SK": $util.dynamodb.toDynamoDBJson($PK),
  },
  "update" : {
    "expression": "$expression"
    #if( !${expNames.isEmpty()} )
      , "expressionNames": $utils.toJson($expNames)
    #end
    #if( !${expValues.isEmpty()} )
      , "expressionValues": $utils.toJson($expValues)
    #end
  },
  "condition": {
    ## Only update if item already exists and UpdatedAt is newer **
    "expression": "attribute_exists(#PK) AND #UpdatedAt < :UpdatedAt",
    "expressionNames": {
      "#PK": "PK",
      "#UpdatedAt": "UpdatedAt"
    },
    "expressionValues": {
      ":UpdatedAt": $utils.dynamodb.toDynamoDBJson($UpdatedAt)
    }
  }
}
this is the mutation for call category

