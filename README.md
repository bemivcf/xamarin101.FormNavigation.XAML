# xamarin101.FormNavigation.XAML

# MainPage.xaml.cs
```
 public class MainPageViewModel : INotifyPropertyChanged
 {
      
        public ObservableCollection<string> Notes { get; set; } 

        public event PropertyChangedEventHandler PropertyChanged;
        string theNote;
        string selectedNote;
        public string SelectedNote {
            get => selectedNote;
            set {
                selectedNote = value;
                var args = new PropertyChangedEventArgs(nameof(SelectedNote));
                PropertyChanged?.Invoke(this, args);
            }
        }
        public string TheNote
        {
            get => theNote;
            set
            {
                theNote = value;
                var args = new PropertyChangedEventArgs(nameof(TheNote));
                PropertyChanged?.Invoke(this, args);
            }
        }

        public Command SaveCommand { get; }
        public Command EraseCommand { get; }
        public Command SelectedNoteChangedCommand { get; }

        public MainPageViewModel()
        {
            Notes = new ObservableCollection<string>();

            SelectedNoteChangedCommand = new Command(async() =>
            {
                var detailVM = new DetailPageViewModel(SelectedNote);
                var detailPage = new DetailPage();
                detailPage.BindingContext = detailVM;

                await Application.Current.MainPage.Navigation.PushAsync(detailPage);
            });

            EraseCommand = new Command(() => { TheNote = string.Empty; });

            SaveCommand = new Command(()=> {
                Notes.Add(TheNote);

                TheNote = string.Empty;
            });
        }

      
    }
```
# MainPage.xaml
```
...
<CollectionView ItemsSource="{Binding Notes}"
                        SelectionMode="Single"
                        SelectedItem="{Binding SelectedNote}"
                        SelectionChangedCommand="{Binding SelectedNoteChangedCommand}"
                        
                        Grid.Row="3" 
                        Grid.ColumnSpan="2">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <StackLayout Padding="10,10">
                        <Frame>
                            <Label Text="{Binding .}" VerticalTextAlignment="Center"></Label>
                        </Frame>
                    </StackLayout>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
...
```
# DetailPageViewModel.cs
```
 public class DetailPageViewModel: INotifyPropertyChanged
    {
        public event PropertyChangedEventHandler PropertyChanged;

        public DetailPageViewModel(string note) {
            NoteText = note;
            DismissPageCommand = new Command(async() => {
                await Application.Current.MainPage.Navigation.PopAsync();
            });
        }

        string noteText;
        public string NoteText 
        {
            get => noteText;
            set
            {
                noteText = value;
                var args = new PropertyChangedEventArgs(nameof(NoteText));
                PropertyChanged?.Invoke(this, args);
            }
        }
        public Command DismissPageCommand { get; }

    }
 ```   
# Detailpage.xaml
```
...
<ContentPage.Content>
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="*"></RowDefinition>
                <RowDefinition Height=".2*"/>
            </Grid.RowDefinitions>

            <Label Text="{Binding NoteText}"
                   FontSize="Title"
                   Grid.Row="0"
                   VerticalOptions="CenterAndExpand"
                   HorizontalOptions="CenterAndExpand"></Label>
            <Button Grid.Row="1"
                    Text="Dismiss"
                    Command="{Binding DismissPageCommand}"></Button>
        </Grid>
        
    </ContentPage.Content>
...
```
# MainPageViewModel.cs 
```
...
 public class MainPageViewModel : INotifyPropertyChanged
    {
        //collection
        public ObservableCollection<string> Notes { get; set; } // create a list at anytime it changes; let the list updated

        public event PropertyChangedEventHandler PropertyChanged;
        string theNote;
        string selectedNote;
        public string SelectedNote {
            get => selectedNote;
            set {
                selectedNote = value;
                var args = new PropertyChangedEventArgs(nameof(SelectedNote));
                PropertyChanged?.Invoke(this, args);
            }
        }
        public string TheNote
        {
            get => theNote;
            set
            {
                theNote = value;
                var args = new PropertyChangedEventArgs(nameof(TheNote));
                PropertyChanged?.Invoke(this, args);
            }
        }

        public Command SaveCommand { get; }
        public Command EraseCommand { get; }
        public Command SelectedNoteChangedCommand { get; }

        public MainPageViewModel()
        {
            Notes = new ObservableCollection<string>();

            SelectedNoteChangedCommand = new Command(async() =>
            {
                var detailVM = new DetailPageViewModel(SelectedNote);
                var detailPage = new DetailPage();
                detailPage.BindingContext = detailVM;

                await Application.Current.MainPage.Navigation.PushAsync(detailPage);
            });

            EraseCommand = new Command(() => { TheNote = string.Empty; });

            SaveCommand = new Command(()=> {
                Notes.Add(TheNote);

                TheNote = string.Empty;
            });
        }

      
    }
...
```
