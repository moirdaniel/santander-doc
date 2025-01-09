# santander-doc

openapi: '3.0.1'
info:
  title: API Title
  version: '1.0'
servers:
  - url: https://api.server.test/v1

paths:
  /v1/update_invoice:
    post:
      description: ''
      parameters: 
        - name: x-schema-id
          description: schema id
          schema:
            type: string
          in: header
          required: true
        - name: x-santander-client-id
          description: "Client ID header"
          example: "a1b30a84-7bf3-442e-84a0-e935d8163b5a"
          schema:
            type: string
          in: header
          required: true
        - name: x-terminal-id
          description: terminal
          schema:
            type: string
          in: header
        - name: x-context-id
          description: context name
          schema:
            type: string
          in: header
        - name: x-b3-span-id
          description: flow information uid
          schema:
            type: string
          in: header
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                issueDate:
                  type: string
                  description: Fecha de emisión del documento (DD-MM-YYYY)
                invoiceType:
                  type: string
                  description: Ver Tipo Facturación (*)
                customerAccount:
                  type: string
                  description: Cuenta custodia del cliente al que se le emite la factura
                sequential:
                  type: string
                  description: Correlativo de la factura que recibirá el folio
                user:
                  type: string
                  description: Identificador del usuario que realiza la operación
      responses:
        200:
          description: OK
          content:
            application/json:
              schema:
                type: array
                items:
                $ref: '#/components/schemas/CursorControl'
        400:
          $ref: '#/components/responses/400'
        401:
          $ref: '#/components/responses/401'
        403:
          $ref: '#/components/responses/403'
        404:
          $ref: '#/components/responses/404'
        500:
          $ref: '#/components/responses/500'
        502:
          $ref: '#/components/responses/502'
        503:
          $ref: '#/components/responses/503'
        504:
          $ref: '#/components/responses/504'      
  /v1/get_ftp_params:
    get:
      description: ''
      parameters:
        - name: x-schema-id
          description: schema id
          schema:
            type: string
          in: header
          required: true
        - name: x-santander-client-id
          description: "Client ID header"
          example: "a1b30a84-7bf3-442e-84a0-e935d8163b5a"
          schema:
            type: string
          in: header
          required: true
        - name: x-terminal-id
          description: terminal
          schema:
            type: string
          in: header
        - name: x-context-id
          description: context name
          schema:
            type: string
          in: header
        - name: x-b3-span-id
          description: flow information uid
          schema:
            type: string
          in: header      
        - name: id
          in: query
          required: false
          schema:
            type: string
          description: Description for param1
      responses:
        200:
          description: Successful Operation
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/GetParamsResponse'
        400:
          $ref: '#/components/responses/400'
        401:
          $ref: '#/components/responses/401'
        403:
          $ref: '#/components/responses/403'
        404:
          $ref: '#/components/responses/404'
        500:
          $ref: '#/components/responses/500'
        502:
          $ref: '#/components/responses/502'
        503:
          $ref: '#/components/responses/503'
        504:
          $ref: '#/components/responses/504'
              
components:
  responses:
    400:
      description: Bad Request
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
    401:
      description: Unauthorized
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
    403:
      description: Forbidden
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
    404:
      description: Not Found
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
    500:
      description: Internal Server Error
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
    502:
      description: Bad Gateway
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
    503:
      description: Service Unavailable
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
    504:
      description: Gateway Timeout
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'

  schemas:
    CursorControl:
      type: object
      properties:
        code:
          type: string
          description: Código del resultado
        content:
          type: string
          description: Glosa del resultado
    CursorDatos:
      type: object
      properties:
        id:
          type: string
          description: IDENTIFICADOR PARAMETRO
        name:
          type: string
          description: NOMBRE PARAMETRO
        description:
          type: string
          description: DESCRIPCION PARAMETRO
        content:
          type: string
          description: VALOR PARAMETRO
    GetParamsResponse:
      type: object
      properties:
        cursorDatos:
          type: array
          items:
          $ref: '#/components/schemas/CursorDatos'
        cursorControl:
          $ref: '#/components/schemas/CursorControl'
    Error:
      type: object
      properties:
        code:
          type: string
        message:
          type: string
        level:
          type: string
        description:
            type: string
