OWC settings
==============================================

# Introduction

In this short document are exposed where the OWC components configurations are stored and what are the associated roles.


# Configurations table

| Component     | Description                                        | Role               | User Server | User Local | Global Server |
|---------------|----------------------------------------------------|--------------------|-------------|------------|---------------|
| Menu          | Menu items configuration                           | Administrator      |             |            | X             |
| List settings | Configuration of attributes shown in the item list | Authenticated user |             | X          |               |
| Map settings  | Map Layer configuration                            | Anonymous          |             | X          |               |
| Theme         | Application appaerance                             | Authenticated user |             | X          |               |
| Language      | Application language                               | Anonymous          |             | X          |               |  |


Where

| User Server   | User configuration stored server-side. The user retrieve the saved settings changing browser                                                                |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| User Local    | User configuration stored client-side. The user does not retrieve the saved settings changing browser (the settings are saved in the browser local storage) |
| Global Server | The configurations are saved server-side and affects all users                                                                                              |
