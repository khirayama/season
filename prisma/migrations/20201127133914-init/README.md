# Migration `20201127133914-init`

This migration has been generated by Kotaro Hirayama at 11/27/2020, 10:39:14 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE "Month" (
    "id" TEXT NOT NULL,
    "month" INTEGER NOT NULL,
    "name" TEXT NOT NULL,
    "kana" TEXT NOT NULL,
    "createdAt" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "updatedAt" DATETIME NOT NULL,
    "ingredientId" TEXT,

    FOREIGN KEY ("ingredientId") REFERENCES "Ingredient"("id") ON DELETE SET NULL ON UPDATE CASCADE,
    PRIMARY KEY ("id")
)

CREATE TABLE "Rokuyo" (
    "id" TEXT NOT NULL,
    "name" TEXT NOT NULL,
    "kana" TEXT NOT NULL,
    "note" TEXT NOT NULL,
    "createdAt" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "updatedAt" DATETIME NOT NULL,
    PRIMARY KEY ("id")
)

CREATE TABLE "Calendarday" (
    "id" TEXT NOT NULL,
    "label" TEXT NOT NULL,
    "name" TEXT NOT NULL,
    "kana" TEXT NOT NULL,
    "note" TEXT NOT NULL,
    "createdAt" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "updatedAt" DATETIME NOT NULL,
    PRIMARY KEY ("id")
)

CREATE TABLE "Calendar" (
    "id" TEXT NOT NULL,
    "date" DATETIME NOT NULL,
    "createdAt" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "updatedAt" DATETIME NOT NULL,
    "rokuyoId" TEXT NOT NULL,

    FOREIGN KEY ("rokuyoId") REFERENCES "Rokuyo"("id") ON DELETE CASCADE ON UPDATE CASCADE,
    PRIMARY KEY ("id")
)

CREATE TABLE "Schedule" (
    "id" TEXT NOT NULL,
    "date" DATETIME NOT NULL,
    "name" TEXT NOT NULL,
    "createdAt" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "updatedAt" DATETIME NOT NULL,
    "calendardayId" TEXT NOT NULL,

    FOREIGN KEY ("calendardayId") REFERENCES "Calendarday"("id") ON DELETE CASCADE ON UPDATE CASCADE,
    PRIMARY KEY ("id")
)

CREATE TABLE "Ingredient" (
    "id" TEXT NOT NULL,
    "label" TEXT NOT NULL,
    "name" TEXT NOT NULL,
    "kana" TEXT NOT NULL,
    "note" TEXT NOT NULL,
    "createdAt" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "updatedAt" DATETIME NOT NULL,
    PRIMARY KEY ("id")
)
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration ..20201127133914-init
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,81 @@
+// https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference
+// https://www.prisma.io/docs/getting-started/setup-prisma/start-from-scratch-prisma-migrate-typescript-postgres
+
+datasource db {
+  provider = "sqlite"
+  url = "***"
+}
+
+generator client {
+  provider = "prisma-client-js"
+}
+
+model Month {
+  id          String   @id @default(uuid())
+  month       Int
+  name        String
+  kana        String
+  createdAt   DateTime @default(now())
+  updatedAt   DateTime @updatedAt
+}
+
+model Rokuyo {
+  id        String   @id @default(uuid())
+  name      String
+  kana      String
+  note      String
+  createdAt DateTime @default(now())
+  updatedAt DateTime @updatedAt
+}
+
+//enum CalendardayLabel {
+//  NATIONALHOLIDAY
+//  SOLARTERM
+//  SPECIALTERM
+//}
+
+model Calendarday {
+  id        String   @id @default(uuid())
+  label     String // CalendardayLabel
+  name      String
+  kana      String
+  note      String
+  createdAt DateTime @default(now())
+  updatedAt DateTime @updatedAt
+}
+
+model Calendar {
+  id        String   @id @default(uuid())
+  date      DateTime
+  rokuyo    Rokuyo
+  createdAt DateTime @default(now())
+  updatedAt DateTime @updatedAt
+}
+
+model Schedule {
+  id          String   @id @default(uuid())
+  date        DateTime
+  name        String
+  calendarday Calendarday
+  createdAt   DateTime @default(now())
+  updatedAt   DateTime @updatedAt
+}
+
+//enum IngredientLabel {
+//  VEGETABLE
+//  FISH
+//  FRUIT
+//  SEAFOOD
+//  OTHER
+//}
+
+model Ingredient {
+  id        String          @id @default(uuid())
+  label     String // IngredientLabel
+  name      String
+  kana      String
+  note      String
+  season    Month[]
+  createdAt DateTime        @default(now())
+  updatedAt DateTime        @updatedAt
+}
```

