<template>
    <div>
       <v-snackbar v-model="snackbar">
            {{ message[code] }}
            <v-btn color="pink" text v-bind="attrs" @click="snackbar = !snackbar">Close</v-btn>
        </v-snackbar>
  </div>
</template>

<script>
    export default {
        name: 'http_msg',
        props: {
          code: Number
        },
        data:()=>{
            return{
                snackbar: true,
                message:{
                  200:'200 OK, Успешно выполнено', 
                  201:'201 CREATED, Создано',
                  204:'204 OK, Успешно выполнено',
                  400:'400 BAD REQUEST, Cерверу непонятен запрос от клиента.',
                  401:'401 UNAUTHORIZED.',
                  403:'403 FORBIDDEN, Операция запрещена для текущего пользователя.',
                  404:'404 NOT FOUND, В запросе был указан неизвестный entity или id несуществующего объекта.',
                  409:'409 CONFLICT, Запрос конфликтует с текущим состоянием сервера.',
                  412:'412 PRECONDITION FAILED, Клиент указал в своих заголовках условия, которые сервер не может выполнить',
                  415:'415 Unsupported Media Type Фактический формат переданного файла не поддерживается.',
                  418:'418 I\'m a Teapot Неизвестный серверу запрос, который не удалось даже разобрать.',
                  419:'419 Authentication Timeout Необходимо пройти повторную авторизацию.',
                  422:'422 Unprocessable Entity Запрос корректно разобран, но содержание запроса не прошло серверную валидацию.',
                  500:'500 Internal Server Error На сервере вылетело необработанное исключение или произошла другая необработанная ошибка.',
                  501:'501 Not Implemented Текущий метод неприменим (не реализован) к объекту запроса.'
                }
            }
        }
    }
</script>