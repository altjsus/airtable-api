# The Airtable API for Haskell

Provides a high-level interface to requesting and introspecting Tables within an Airtable project using Lens, Wreq, and Aeson.  
First version was created by @ooblahman and it has problem with recent GHC. Semigroup is a superclass of Monoid since `base-4.11.0.0`. That causes error on `stack install` and manual compilation fails if base is of recent version.

## Example

```haskell
import Data.Text
import Airtable.Table
import Airtable.Query
import Data.Aeson
import Data.Aeson.Types

settings = AirtableOptions "key" "app" 0

data TestAirtableObject = TestAirtableObject {
  name :: Text
} deriving (Eq, Show)

instance FromJSON TestAirtableObject where
parseJSON = withObject "TestAirtableObject" $ \v -> TestAirtableObject
	<$> v .: (pack "Name") -- or {-# LANGUAGE OverloadedStrings #-} can be used not to use "pack" all the time

main :: IO ()
main = do
  x <- getRecords settings "TestAirtableObjects" :: IO (Table TestAirtableObject)
  print x
```