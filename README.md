# Tau release validation suite

## Setup

Clone this repository within a CMSSW environment:

    git clone https://github.com/cms-tau-pog/TauReleaseValidation.git

If you plan to update the TauID list wrt the used CMSSW version do:

     wget https://raw.githubusercontent.com/greyxray/TauAnalysisTools/CMSSW_9_4_X_tau-pog_RunIIFall17/TauAnalysisTools/python/runTauIdMVA.py  -P $CMSSW_BASE/src/TauReleaseValidation/

Run the following commands to produce validation plots:

     python produceTauValTree.py ZTT
     python compare.py ZTT

## Things to do/notes

* Uses eostools from cmg-cmssw since the one in CMSSW is broken
* Add more samples (VBF Higgs, H+, ZEE)
* Finish setting up the different samples

## Example 1 - single release of cmssw 9

dataset: /RelValZTT_13/CMSSW_9_4_0-PUpmx25ns_94X_mc2017_realistic_v10-v1/MINIAODSIM

Copy the samples from das using xrdcp in your eos space, the path can be created for example with command: `mkdir -p /eos/user/o/ohlushch/relValMVA/RelValZTT_13/CMSSW_9_4_0-PUpmx25ns_94X_mc2017_realistic_v10-v1/MINIAODSIM/`.

If one wants to re-run the tau-id sequence in order to access other trainings than ones provided with the files run the produceEDMNtupleMiniAODwithUpdatedTauID_cfg.py:
	voms-proxy-init -voms cms -rfc
	cmsRun produceEDMNtupleMiniAODwithUpdatedTauID_cfg.py storageSite=loc localdir='/eos/user/o/ohlushch/relValMVA/' runtype='ZTT' release=CMSSW_9_4_0 globalTag='PUpmx25ns_94X_mc2017_realistic_v10-v1'

So now you have a created new edm ntuple in your folder. In our example it will be `/eos/user/o/ohlushch/relValMVA/RelValZTT_13/CMSSW_9_4_0-PUpmx25ns_94X_mc2017_realistic_v10-v1/TauValTree/Myroot_CMSSW_9_4_0_PUpmx25ns_94X_mc2017_realistic_v10-v1_ZTT.root`. Produce the plain ntuple:
	python produceTauValTree.py --release CMSSW_9_4_0 --globalTag PUpmx25ns_94X_mc2017_realistic_v10-v1 --inputfile /eos/user/o/ohlushch/relValMVA/RelValZTT_13/CMSSW_9_4_0-PUpmx25ns_94X_mc2017_realistic_v10-v1/Updated/output.root --runtype ZTT --mvaid 2017v1 2017v2 newDM2017v2 dR0p32017v2 2016v1 newDM2016v1 --tauCollection NewTauIDsEmbedded

In the example where you don't want to re-run the sequence and have the files locally:
    python produceTauValTree.py -r CMSSW_9_4_0 -g PUpmx25ns_94X_mc2017_realistic_v10-v1 -n 100 -s loc --runtype ZTT --localdir=/eos/user/o/ohlushch/relValMVA/

And run your compare files:
	python compare.py --runtype ZTT --globalTag PUpmx25ns_94X_mc2017_realistic_v10-v1  --releases CMSSW_9_4_0  --part 1 --debug --inputfiles  /eos/user/o/ohlushch/relValMVA/RelValZTT_13/CMSSW_9_4_0-PUpmx25ns_94X_mc2017_realistic_v10-v1/TauValTree/Myroot_CMSSW_9_4_0_PUpmx25ns_94X_mc2017_realistic_v10-v1_ZTT.root

or comperPerRelese.py specifying which MVAs you want to compare:
	python comparePerRelease.py --runtype ZTT --globalTag PUpmx25ns_94X_mc2017_realistic_v10-v1 --release CMSSW_9_4_0  --part 1 --debug --inputfile /eos/user/o/ohlushch/relValMVA/RelValZTT_13/CMSSW_9_4_0-PUpmx25ns_94X_mc2017_realistic_v10-v1/TauValTree/Myroot_CMSSW_9_4_0_PUpmx25ns_94X_mc2017_realistic_v10-v1_ZTT.root -v byLooseIsolationMVArun2v1DBoldDMwLT byLooseIsolationMVArun2v1PWoldDMwLT byLooseIsolationMVArun2017v1DBoldDMwLT2017 byLooseIsolationMVArun2017v2DBoldDMwLT2017 byLooseIsolationMVArun2v1DBoldDMwLT2016 -c 1 3 4 5 7  --varyLooseId

# Example 2 - several releases of cmssw 10

Just an example on how to 
	python produceTauValTree.py -r CMSSW_10_2_0_pre2 -g 101X_mcRun2_asymptotic_v5_FastSim-v1 --runtype ZTT -s eos --localdir=/eos/user/o/ohlushch/relValMVA/ --mvaid

	python compare.py -r CMSSW_10_2_0_pre2 CMSSW_10_2_0_pre2 -g 101X_mcRun2_asymptotic_v5_FastSim-v1 101X_upgrade2018_realistic_v7-v1 --runtype ZTT -n 100 --inputfiles Myroot_CMSSW_10_2_0_pre2_101X_upgrade2018_realistic_v7-v1_ZTT.root  Myroot_CMSSW_10_2_0_pre2_101X_mcRun2_asymptotic_v5_FastSim-v1_ZTT.root

Using wrapper:
	python produceAndCompare.py -r CMSSW_10_2_0_pre2 CMSSW_10_2_0_pre2 -g 101X_mcRun2_asymptotic_v5_FastSim-v1 101X_upgrade2018_realistic_v7-v1 --runtype ZTT -n 100 --inputfiles /afs/cern.ch/user/d/dmroy/public/forolena/Myroot_CMSSW_10_2_0_pre2_101X_upgrade2018_realistic_v7-v1_ZTT.root  /afs/cern.ch/user/d/dmroy/public/forolena/Myroot_CMSSW_10_2_0_pre2_101X_mcRun2_asymptotic_v5_FastSim-v1_ZTT.root

## Few extra notes

* --runtype is obligatory
* --mvaid is used only for re-running of sequence

