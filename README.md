# FileAssociation
File Association for Windows, to use with Lazarus IDE

# Usage
First install the package. You can then drop the component TFileAssociation (FileAssoc unit) that gets installed in the System tab of the IDE on your form.

All parameters are mandatory. Especially ActionName which must be 'Open' to work with double click. This is useful as well for default commands like 'Edit' and 'Print'. This must be in English for 'Edit', 'Open' and 'Print' so it can access the right registry entry. You can customize the translation with ActionText.

```pascal
...
uses
  ...
  FileAssoc;//<-- add fileassociation unit here

type

...
    { private declarations }
    assoc: TFileAssociation;
...

procedure TfrmMain.FormCreate(Sender: TObject);
begin
  assoc := TFileAssociation.Create(Self);//<-- create like a regular component
end;

procedure TfrmMain.Button1Click(Sender: TObject);
begin
  assoc.ApplicationName := 'Lazarus IDE';
  assoc.ApplicationDescription := 'RAD for Free Pascal';

  // you can change Extension and Action part for each extension you have

  assoc.Extension := '.lpr';
  assoc.ExtensionName := 'Lazarus Project';
  assoc.ExtensionIcon := '"C:\lazarus\images\lprfile.ico"';

  // full path required, you can use ParamStr(0) to get the path with the .exe name included. The path must be inside quotes if it has whitespace.
  assoc.Action := '"C:\lazarus\lazarus.exe" "%1"'; 
  assoc.ActionName := 'Open';
  assoc.ActionIcon := '"C:\lazarus\images\mainicon.ico"';

  // notice that using RegisterForAllUsers as True requires Administrator Privileges
  // if you want to run without privileges set it to false, but it will register for current user only
  assoc.RegisterForAllUsers:=False;
  if assoc.Execute then
  begin
    ShowMessage('OK');
    assoc.ClearIconCache; //<<-- rebuild icons
  end;
end;

end.
```

# How to open the associated file

```pascal
procedure TForm1.FormCreate(Sender: TObject);
var
  s: String;
begin
// if there are parameters
  if ParamCount > 0 then
  begin
// load the first parameter
    s := ParamStr(1);
 
// if is a .txt file
    if ExtractFileExt(s) = '.txt' then
    begin
// load the .txt file into a memo
      Memo1.Lines.LoadFromFile(s);
    end;
  end;
end;
```

# License
Modified LGPL (also referred to as FPC modified LGPL) is the Library GNU General Public License with the following modification:
As a special exception, the copyright holders of this library give you permission to link this library with independent modules to produce an executable, regardless of the license terms of these independent modules, and to copy and distribute the resulting executable under terms of your choice, provided that you also meet, for each linked independent module, the terms and conditions of the license of that module. An independent module is a module which is not derived from or based on this library. If you modify this library, you may extend this exception to your version of the library, but you are not obligated to do so. If you do not wish to do so, delete this exception statement from your version.
