- [Mastering Android ViewModels: Essential Dos and Don’ts Part 1](https://proandroiddev.com/mastering-android-viewmodels-essential-dos-and-donts-part-1-%EF%B8%8F-bdf05287bca9)
- [CloseableCoroutineScope to use intead of viewModelScope](https://developer.android.com/jetpack/androidx/releases/lifecycle#2.5.0)
- [How to create View Model without Android architecture component](https://stackoverflow.com/a/76306457/9636037)
- [viewModelScope injection](https://developer.android.com/jetpack/androidx/releases/lifecycle#2.8.0-alpha03)

### Resources in ViewModel
[Locale changes and the AndroidViewModel antipattern](https://medium.com/androiddevelopers/locale-changes-and-the-androidviewmodel-antipattern-84eb677660d9)

Marcel Pinto's comment in the same blog

> I would even argue the use of resource ids in the ViewModel. I’d rather delegate to the view the decision of which string/color/value to represent. The ViewModel should only provide the needed values so the view can create the resource.

