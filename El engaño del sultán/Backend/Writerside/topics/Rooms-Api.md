# Rooms-Api

## Introduction
`Rooms-Api` is a NestJs module responsible for controlling the layout and template of each sultan room.
This `Api` provides endpoints that allow users to interact with rooms. There are different types of `rooms`
and number of `doors`, allowing to create a wide range of rooms. This allows to have a more precise control
over the different data that is stored in each room. Additionally, there are different types of doors
with different states to follow the flow of users and how they interact with the application.

## Entities

### Doors {collapsible="true"}
The `Doors` entity represents the doors in the application, each door having specific attributes and
relationships.

#### Properties {id="door-properties"}
<code-block lang="typescript">
    @PrimaryGeneratedColumn()
    id_sultan_door: number;
</code-block>
- **id_sultan_door**: *(PrimaryGeneratedColumn)*
  - Data Type: `number`
  - Description: Unique identifier for each door.

<code-block lang="typescript">
    @Column({ nullable: false })
    sultan_door_type: string;
</code-block>
- **sultan_door_type**: *(Column)*
  - Data Type: `string`
  - Description: Specifies the type or category of the door.
  - Restrictions:
    - This column can not be null in the database.

<code-block lang="typescript">
    @Column({ nullable: false })
    sultan_door_state: string;
</code-block>
- **sultan_door_state**: *(Column)*
  - Data Type: `string`
  - Description: Represents the current state or condition of the door.
  - Restrictions:
    - This column can not be null in the database.

<code-block lang="typescript">
    @ManyToOne(() => Rooms, (sultanRoom) => sultanRoom.id_sultan_doors) // Relación muchos a uno con las habitaciones
    @JoinColumn({ name: "id_sultan_room" }) // Nombre de la columna en la tabla de puertas que almacena el ID de la habitación
    id_sultan_room: Rooms;
</code-block>
- **id_sultan_room**: *(ManyToOne Relationship)*
  - Data Type: `Rooms`
  - Description: Establishes a many-to-one relationship between doors and rooms, indicating which room the door belongs to.

<code-block lang="typescript">
    @CreateDateColumn({
        type: "timestamp",
        default: () => "CURRENT_TIMESTAMP(6)",
    })
    public created_at: Date;
</code-block>
- **created_at**: *(CreateDateColumn)*
  - Data Type: `Date`
  - Description: Indicates the timestamp when the door entity was created.

<code-block lang="typescript">
    @UpdateDateColumn({
        type: "timestamp",
        default: () => "CURRENT_TIMESTAMP(6)",
        onUpdate: "CURRENT_TIMESTAMP(6)",
    })
    public updated_at: Date;
</code-block>
- **updated_at**: *(UpdateDateColumn)*
  - Data Type: `Date`
  - Description: Reflects the timestamp when the door entity was last updated.

#### Relationships {id="doors-relationships"}

- **Rooms**: *(ManyToOne Relationship)*
  - Description: Each door is associated with a specific room in the application.
  - Foreign Key: `id_sultan_room`
  - Type: `Rooms`

#### Data Transfer Objects {id="doors-data-transfer-objects"}
##### CreateDoorsDto {id="doors-dto"}
The `CreateDoorsDto` class defines a data transfer object (DTO) used for creating new door element
the door entity.

###### Class Fields {id="doors-class-fields"}

- **roomId**: *(number)*
  - Validation: Not empty, positive number.
  - Description: Represents the ID of the room to which the door belongs.
- **doorType**: *(string)*
  - Validation: Not empty, enumeration of possible door types (enum: `DoorTypes`).
  - Description: Specifies the type or category of the door.
- **doorState**: *(string)*
  - Validation: Not empty, enumeration of possible door states (enum: `DoorStates`).
  - Description: Represents the current state or condition of the door.

###### Validation {id="doors-validation"}

- **roomId**:
  - Must not be empty.
  - Must be a positive number.
- **doorType**:
  - Must not be empty.
  - Must be one of the values defined in the `DoorTypes` enumeration.
- **doorState**:
  - Must not be empty.
  - Must be one of the values defined in the `DoorStates` enumeration.

<tabs>
    <tab title="payload">
        <code-block lang="json">
            {
              "roomId": 3,
              "doorType": "exercise",
              "doorState": "enable"
            }
        </code-block>
    </tab>
    <tab title="schema">
        <code-block lang="typescript">
            export class CreateDoorsDto {
                @IsNotEmpty()
                @IsPositive()
                @ApiProperty()
                roomId: number;
                @IsNotEmpty()
                @IsEnum(DoorTypes)
                @ApiProperty({ enum: DoorTypes, enumName: "DoorTypes" })
                doorType: string;
                @IsNotEmpty()
                @IsEnum(DoorStates)
                @ApiProperty({ enum: DoorStates, enumName: "DoorStates" })
                doorState: string;
            }
        </code-block>
    </tab>
</tabs>

#### DoorTypes Enum {id="doors-types-enum"}
The `DoorTypes` enum defines the possible types or categories of doors in the application.

- **module**: Represents a door associated with a module.
- **exercise**: Represents a door associated with an exercise.

<code-block lang="typescript">
export enum DoorTypes {
    module = "Module",
    exercise = "Exercise",
}
</code-block>

#### DoorStates Enum {id="doors-states-enum"}
The `DoorStates` enum defines the possible states or conditions of doors in the application.

- **enable**: Indicates that the door is enabled or active.
- **processed**: Indicates that the door has been processed or completed.
- **unprocessed**: Indicates that the door has not been processed or completed.
- **disable**: Indicates that the door is disabled or inactive.

<code-block lang="typescript">
export enum DoorStates {
    enable = "Enable",
    processed = "Processed",
    unprocessed = "Unprocessed",
    disable = "Disable",
}
</code-block>


#### Notes {id="doors-notes"}

- The `Doors` entity stores information about individual doors in the application.
- It includes properties for identifying each door, such as its type and state, as well as timestamps for creation and updates.
- Additionally, it establishes a relationship with the `Rooms` entity to denote which room each door belongs to.

<tabs>
  <tab title="Table Example">
    <table>
      <tr>
        <td>id_sultan_door</td>
        <td>sultan_door_type</td>
        <td>sultan_door_state</td>
        <td>id_sultan_room</td>
        <td>created_at</td>
        <td>updated_at</td>
      </tr>
      <tr>
        <td>1</td>
        <td>Module</td>
        <td>Disable</td>
        <td>1</td>
        <td>2024-04-25 15:06:15.477171</td>
        <td>2024-04-25 15:06:15.477171</td>
      </tr>
      <tr>
        <td>2</td>
        <td>Exercise</td>
        <td>Enable</td>
        <td>2</td>
        <td>2024-04-25 15:06:15.477171</td>
        <td>2024-04-25 15:06:15.477171</td>
      </tr>
      <tr>
        <td>3</td>
        <td>Exercise</td>
        <td>Enable</td>
        <td>2</td>
        <td>2024-04-25 15:06:15.477171</td>
        <td>2024-04-25 15:06:15.477171</td>
      </tr>
    </table>
  </tab>
  <tab title="Schema">
    <code-block lang="typescript">
      @Entity()
      export class Doors {
          @PrimaryGeneratedColumn()
          id_sultan_door: number;
          @Column({ nullable: false })
          sultan_door_type: string;
          @Column({ nullable: false })
          sultan_door_state: string;
          @ManyToOne(() => Rooms, (sultanRoom) => sultanRoom.id_sultan_doors)
          @JoinColumn({ name: "id_sultan_room" })
          id_sultan_room: Rooms;
          @CreateDateColumn({
              type: "timestamp",
              default: () => "CURRENT_TIMESTAMP(6)",
          })
          public created_at: Date;
          @UpdateDateColumn({
              type: "timestamp",
              default: () => "CURRENT_TIMESTAMP(6)",
              onUpdate: "CURRENT_TIMESTAMP(6)",
          })
          public updated_at: Date;
      }
    </code-block>
  </tab>
</tabs>


### RoomsType {collapsible="true"}
The `Doors` entity represents the doors in the application, each door having specific attributes and
relationships.

#### Properties {id="rooms-type-properties"}
<code-block lang="typescript">
    @PrimaryGeneratedColumn()
    id_sultan_door: number;
</code-block>
- **id_sultan_door**: *(PrimaryGeneratedColumn)*
  - Data Type: `number`
  - Description: Unique identifier for each door.

<code-block lang="typescript">
    @Column({ nullable: false })
    sultan_door_type: string;
</code-block>
- **sultan_door_type**: *(Column)*
  - Data Type: `string`
  - Description: Specifies the type or category of the door.
  - Restrictions:
    - This column can not be null in the database.

<code-block lang="typescript">
    @Column({ nullable: false })
    sultan_door_state: string;
</code-block>
- **sultan_door_state**: *(Column)*
  - Data Type: `string`
  - Description: Represents the current state or condition of the door.
  - Restrictions:
    - This column can not be null in the database.

<code-block lang="typescript">
    @ManyToOne(() => Rooms, (sultanRoom) => sultanRoom.id_sultan_doors) // Relación muchos a uno con las habitaciones
    @JoinColumn({ name: "id_sultan_room" }) // Nombre de la columna en la tabla de puertas que almacena el ID de la habitación
    id_sultan_room: Rooms;
</code-block>
- **id_sultan_room**: *(ManyToOne Relationship)*
  - Data Type: `Rooms`
  - Description: Establishes a many-to-one relationship between doors and rooms, indicating which room the door belongs to.

<code-block lang="typescript">
    @CreateDateColumn({
        type: "timestamp",
        default: () => "CURRENT_TIMESTAMP(6)",
    })
    public created_at: Date;
</code-block>
- **created_at**: *(CreateDateColumn)*
  - Data Type: `Date`
  - Description: Indicates the timestamp when the door entity was created.

<code-block lang="typescript">
    @UpdateDateColumn({
        type: "timestamp",
        default: () => "CURRENT_TIMESTAMP(6)",
        onUpdate: "CURRENT_TIMESTAMP(6)",
    })
    public updated_at: Date;
</code-block>
- **updated_at**: *(UpdateDateColumn)*
  - Data Type: `Date`
  - Description: Reflects the timestamp when the door entity was last updated.

#### Relationships {id="rooms-type-relationships"}

- **Rooms**: *(ManyToOne Relationship)*
  - Description: Each door is associated with a specific room in the application.
  - Foreign Key: `id_sultan_room`
  - Type: `Rooms`

#### Data Transfer Objects {id="rooms-type-data-transfer-objects"}
##### CreateDoorsDto {id="rooms-type-dto"}
The `CreateDoorsDto` class defines a data transfer object (DTO) used for creating new door element
the door entity.

###### Class Fields {id="rooms-type-class-fields"}

- **roomId**: *(number)*
  - Validation: Not empty, positive number.
  - Description: Represents the ID of the room to which the door belongs.
- **doorType**: *(string)*
  - Validation: Not empty, enumeration of possible door types (enum: `DoorTypes`).
  - Description: Specifies the type or category of the door.
- **doorState**: *(string)*
  - Validation: Not empty, enumeration of possible door states (enum: `DoorStates`).
  - Description: Represents the current state or condition of the door.

###### Validation {id="rooms-type-validation"}

- **roomId**:
  - Must not be empty.
  - Must be a positive number.
- **doorType**:
  - Must not be empty.
  - Must be one of the values defined in the `DoorTypes` enumeration.
- **doorState**:
  - Must not be empty.
  - Must be one of the values defined in the `DoorStates` enumeration.

<tabs>
    <tab title="payload">
        <code-block lang="json">
            {
              "roomId": 3,
              "doorType": "exercise",
              "doorState": "enable"
            }
        </code-block>
    </tab>
    <tab title="schema">
        <code-block lang="typescript">
            export class CreateDoorsDto {
                @IsNotEmpty()
                @IsPositive()
                @ApiProperty()
                roomId: number;
                @IsNotEmpty()
                @IsEnum(DoorTypes)
                @ApiProperty({ enum: DoorTypes, enumName: "DoorTypes" })
                doorType: string;
                @IsNotEmpty()
                @IsEnum(DoorStates)
                @ApiProperty({ enum: DoorStates, enumName: "DoorStates" })
                doorState: string;
            }
        </code-block>
    </tab>
</tabs>

#### DoorTypes Enum {id="rooms-type-types-enum"}
The `DoorTypes` enum defines the possible types or categories of doors in the application.

- **module**: Represents a door associated with a module.
- **exercise**: Represents a door associated with an exercise.

<code-block lang="typescript">
export enum DoorTypes {
    module = "Module",
    exercise = "Exercise",
}
</code-block>

#### DoorStates Enum {id="rooms-type-states-enum"}
The `DoorStates` enum defines the possible states or conditions of doors in the application.

- **enable**: Indicates that the door is enabled or active.
- **processed**: Indicates that the door has been processed or completed.
- **unprocessed**: Indicates that the door has not been processed or completed.
- **disable**: Indicates that the door is disabled or inactive.

<code-block lang="typescript">
export enum DoorStates {
    enable = "Enable",
    processed = "Processed",
    unprocessed = "Unprocessed",
    disable = "Disable",
}
</code-block>


#### Notes {id="rooms-type-notes"}

- The `Doors` entity stores information about individual doors in the application.
- It includes properties for identifying each door, such as its type and state, as well as timestamps for creation and updates.
- Additionally, it establishes a relationship with the `Rooms` entity to denote which room each door belongs to.

<tabs>
  <tab title="Table Example">
    <table>
      <tr>
        <td>id_sultan_door</td>
        <td>sultan_door_type</td>
        <td>sultan_door_state</td>
        <td>id_sultan_room</td>
        <td>created_at</td>
        <td>updated_at</td>
      </tr>
      <tr>
        <td>1</td>
        <td>Module</td>
        <td>Disable</td>
        <td>1</td>
        <td>2024-04-25 15:06:15.477171</td>
        <td>2024-04-25 15:06:15.477171</td>
      </tr>
      <tr>
        <td>2</td>
        <td>Exercise</td>
        <td>Enable</td>
        <td>2</td>
        <td>2024-04-25 15:06:15.477171</td>
        <td>2024-04-25 15:06:15.477171</td>
      </tr>
      <tr>
        <td>3</td>
        <td>Exercise</td>
        <td>Enable</td>
        <td>2</td>
        <td>2024-04-25 15:06:15.477171</td>
        <td>2024-04-25 15:06:15.477171</td>
      </tr>
    </table>
  </tab>
  <tab title="Schema">
    <code-block lang="typescript">
      @Entity()
      export class Doors {
          @PrimaryGeneratedColumn()
          id_sultan_door: number;
          @Column({ nullable: false })
          sultan_door_type: string;
          @Column({ nullable: false })
          sultan_door_state: string;
          @ManyToOne(() => Rooms, (sultanRoom) => sultanRoom.id_sultan_doors)
          @JoinColumn({ name: "id_sultan_room" })
          id_sultan_room: Rooms;
          @CreateDateColumn({
              type: "timestamp",
              default: () => "CURRENT_TIMESTAMP(6)",
          })
          public created_at: Date;
          @UpdateDateColumn({
              type: "timestamp",
              default: () => "CURRENT_TIMESTAMP(6)",
              onUpdate: "CURRENT_TIMESTAMP(6)",
          })
          public updated_at: Date;
      }
    </code-block>
  </tab>
</tabs>

## Controllers

## Services