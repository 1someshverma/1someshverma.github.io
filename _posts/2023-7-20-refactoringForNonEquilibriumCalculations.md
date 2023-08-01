---
layout: post
title: Refactoring for Non-equilibrium Calculations
---

I passed the midterm evaluation, Next i have to refactor part of code in vaex which is used in non-equilibrium calculations.

To refactor code for non-equilibrium calculations , I followed the following straegy.I identified which part of the code is different for Vaex and Pandas , then refactored the code Vaex . 
Main functions which I refactored for non-equilibrium calculations are 
- _calc_line_databank()
- calc_populations_noneq_multiTvib
- calc_linestrength_noneq()
- _cutoff_linestrength()
- calculate_pseudo_continum(noneq=True)

For the other functions which are called inside non_eq_spectrum() I had already refactored them before midTerm Evalulations as they were required for equilibrium calculations as well.So , I don't have to refactor these functions:
- _check_line_databank()
- _reinitialize()
- calc_lineshift()

I have to refactor only five files for it .
- bands.py
- base.py
- brodening.py
- arrays.py
- convert.py

Most of the changes are done in base.py and most of these changes are similar to the following code 

```
            if self.dataframe_type == "pandas":
                # Add lower state energy
                df_pcN = df.set_index(["polyl", "wangl", "rankl"])
                df["Evibl"] = df_pcN.index.map(Evib_dict.get).values
                # Add upper state energy
                df_pcN = df.set_index(["polyu", "wangu", "ranku"])
                df["Evibu"] = df_pcN.index.map(Evib_dict.get).values

                return df.loc[:, ["Evibl", "Evibu"]]
            elif self.dataframe_type == "vaex":
                def get_evib(poly, wang, rank, evib, iso_df):
                    if iso_df == iso :
                        return Evib_dict.get(poly, wang, rank)
                    else:
                        return evib
                    
                df["Evibl"] = df.apply(get_evib, [df.polyl, df.wangl, df.rankl, df.Evibl, df.iso])
                df["Evibu"] = df.apply(get_evib, [df.polyu, df.wangu, df.ranku, df.Evibu, df.iso])
            
        if self.dataframe_type == "pandas":
            df["Evibl"] = np.nan
            df["Evibu"] = np.nan

            # multiple-isotopes in database
            if "iso" in df:
                for iso, idx in df.groupby("iso").indices.items():
                    df.loc[idx, ["Evibl", "Evibu"]] = get_Evib_CDSD_pcN_1iso(
                        df.loc[idx], iso
                    )

                    if radis.config["DEBUG_MODE"]:
                        assert (df.loc[idx, "iso"] == iso).all()

            else:
                iso = df.attrs["iso"]
                df.loc[:, ["Evibl", "Evibu"]] = get_Evib_CDSD_pcN_1iso(df, iso)
        elif self.dataframe_type == "vaex":
            df["Evibl"] = vaex.vconstant(np.nan, df.length_unfiltered())
            df["Evibu"] = vaex.vconstant(np.nan, df.length_unfiltered())

            for iso in list(df.iso.unique()):
                get_Evib_CDSD_pcN_1iso(df, iso)
```

For the broadening.py file I made changes similar to code snippet 
```
            if isinstance(gamma_lb, np.ndarray):
                gamma_lb = gamma_lb.values.reshape((1, -1))def true_for_all(E):
    if E.unique() == [ True ]:
        return True
    else:
        return False
    
def false_for_all(E):
    if E.unique() == [False]:
        return True
    else:
        return False
            elif self.dataframe_type == "vaex":
                gamma_lb = gamma_lb.to_numpy().reshape((1, -1))
```

I also have to make changes to convert.py and added two new functions to it .
```
def true_for_all(E):
    if E.unique() == [ True ]:
        return True
    else:
        return False
    
def false_for_all(E):
    if E.unique() == [False]:
        return True
    else:
        return False
```

These functions helped it checking if some conditions is True for all rows of a dataframe or False for all rows of a dataframe.

For some fucntions i added an extra parameter dataframe_type to check for dataframe type and perform the operations accordingly.