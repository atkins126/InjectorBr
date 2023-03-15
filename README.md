[![InjectorBr Framework](https://www.isaquepinheiro.com.br/imagens/injectorbr_github.png)](https://www.isaquepinheiro.com.br)
<p align="right" width="50%">
    <a href="https://pag.ae/bglQrWD"><img src="https://www.isaquepinheiro.com.br/imagens/doepagueseguro.png"> 
</p>

# InjectorBr Framework for Delphi   [![License](https://img.shields.io/badge/Licence-LGPL--3.0-blue.svg)](https://opensource.org/licenses/LGPL-3.0)
APPInjector Brasil - Dependency Injection for Delphi

# :hammer: Recuros de injeção de dependência com InjectorBr :

:heavy_check_mark: `Recurso 1`: Injector.Register<TClass> (Class)
:heavy_check_mark: `Recurso 2`: Injector.RegisterLazy<TClass> (Class for LazyLoad)
:heavy_check_mark: `Recurso 3`: InjectorInterface<IInterface> (Interface)

Além dessas três formas o framework ainda oferece o recurso de criar uma nova instância de uma classe que já esteja registras, para isso basta usar o comando:
 - Injector<TClass>.New;

### Instalação ###
O InjectorBr não precisa ser instalado, basta adicionar as units no seu projeto e começar a usa-lo.

### Requisitos ###
Embarcadero Delphi XE e superior.

### Versão Atual ###
0.2023.3.15 (15 Mar 2023)

Copyright (c) 2023 InjectorBr Framework Team

# Como usar - Interface ?


```Delphi
{ /////////////////////// Registrando ///////////////////////// }

unit dfe.engine.acbr;

interface

uses
  SysUtils,
  dfe.engine.interfaces;

type
  TDFeEngineACBr = class(TInterfacedObject, IDFeEngine)
  public
    class function New: IDFeEngine;
    procedure Execute;
  end;

implementation

{ TDFeEngineACBr }

procedure TDFeEngineACBr.Execute;
begin
  raise Exception.Create('DFe Engine ACBr');
end;

class function TDFeEngineACBr.New: IDFeEngine;
begin
  Result := Self.Create;
end;

initialization
  InjectorBr.RegisterInterface<IDFeEngine, TDFeEngineACBr>;

end.

{ /////////////////////// Recuperando ///////////////////////// }

unit global.controller;

interface

uses
  DB,
  Rtti,
  Classes,
  SysUtils,
  Controls,
  global.controller.interfaces,
  dfe.engine.interfaces;

type
  TGlobalController = class(TInterfacedObject, IGlobalController)
  private
    FDFeEngine: IDFeEngine;
  public
    constructor Create;
    procedure DFeExecute;
  end;

implementation

uses
  app.injector;

{ TGlobalController }

constructor TGlobalController.Create;
begin
  inherited;
  FDFeEngine := InjectorBr.GetInterface<IDFeEngine>;
end;

procedure TGlobalController.DFeExecute;
begin
  FDFeEngine.Execute;
end;

end.
```
# Como usar - Classe ?

```Delphi
{ /////////////////////// Registrando ///////////////////////// }

unit dfe.engine.acbr;

interface

uses
  SysUtils;

type
  TDFeEngineACBr = class
  public
    procedure Execute;
  end;

implementation

{ TDFeEngineACBr }

procedure TDFeEngineACBr.Execute;
begin
  raise Exception.Create('DFe Engine ACBr');
end;

initialization
  InjectorBr.RegisterSington<TDFeEngineACBr>;

end.

{ /////////////////////// Recuperando ///////////////////////// }

unit global.controller;

interface

uses
  DB,
  Rtti,
  Classes,
  SysUtils,
  Controls,
  global.controller.interfaces,
  dfe.engine.acbr;

type
  TGlobalController = class(TInterfacedObject, IGlobalController)
  private
    FDFeEngine: TDFeEngineACBr;
  public
    constructor Create;
    procedure DFeExecute;
  end;

implementation

uses
  app.injector;

{ TGlobalController }

constructor TGlobalController.Create;
begin
  inherited;
  FDFeEngine := InjectorBr.Get<TDFeEngineACBr>;
end;

procedure TGlobalController.DFeExecute;
begin
  FDFeEngine.Execute;
end;

end.
```

# Como usar - Classe LazyLoad ?

```Delphi
{ /////////////////////// Registrando ///////////////////////// }

unit dfe.engine.acbr;

interface

uses
  SysUtils;

type
  TDFeEngineACBr = class
  public
    procedure Execute;
  end;

implementation

{ TDFeEngineACBr }

procedure TDFeEngineACBr.Execute;
begin
  raise Exception.Create('DFe Engine ACBr');
end;

initialization
  InjectorBr.RegisterLazy<TDFeEngineACBr>;

end.

{ /////////////////////// Recuperando ///////////////////////// }

unit global.controller;

interface

uses
  DB,
  Rtti,
  Classes,
  SysUtils,
  Controls,
  global.controller.interfaces,
  dfe.engine.acbr;

type
  TGlobalController = class(TInterfacedObject, IGlobalController)
  private
    FDFeEngine: TDFeEngineACBr;
  public
    constructor Create;
    procedure DFeExecute;
  end;

implementation

uses
  app.injector;

{ TGlobalController }

constructor TGlobalController.Create;
begin
  inherited;
  FDFeEngine := InjectorBr.Get<TDFeEngineACBr>;
end;

procedure TGlobalController.DFeExecute;
begin
  FDFeEngine.Execute;
end;

end.
```